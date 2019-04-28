# 第1章 发现端点(Discovery Endpoint)
[OpenID Connect发现端点](https://openid.net/specs/openid-connect-discovery-1_0.html)的客户端库作为httpclient的扩展方法提供。该`GetDiscoveryDocumentAsync`方法返回一个`DiscoveryResponse`对象，该对象具有发现文档的各种元素的强类型和弱类型访问器。

在访问文档内容之前，应始终检查`IsError`和`Error`属性。

例：

``` C#
var client = new HttpClient();

var disco = await client.GetDiscoveryDocumentAsync("https://demo.identityserver.io");
if (disco.IsError) throw new Exception(disco.Error);
```   

可以使用以下属性访问标准元素：

``` C#
var tokenEndpoint = disco.TokenEndpoint;
var keys = disco.KeySet.Keys;
```  

自定义元素（或标准属性未涵盖的元素）可以像这样访问：

``` C#
// returns string or null
var stringValue = disco.TryGetString("some_string_element");

// return a nullable boolean
var boolValue = disco.TryGetBoolean("some_boolean_element");

// return array (maybe empty)
var arrayValue = disco.TryGetStringArray("some_array_element");

// returns JToken
var rawJson = disco.TryGetValue("some_element);
```  

## 1.1 发现政策
默认情况下，发现响应在返回到客户端之前已经过验证，验证包括：

* 强制使用HTTPS（localhost地址除外）
* 强制发行人与当局匹配
* 强制协议端点与权限位于同一DNS名称上
* 强制执行密钥集的存在  

可以使用`DiscoveryPolicy`类修改所有标准验证规则，例如禁用颁发者名称检查：

``` C#
var disco = await client.GetDiscoveryDocumentAsync(new DiscoveryDocumentRequest
{
    Address = "https://demo.identityserver.io",
    Policy =
    {
        ValidateIssuerName = false
    }
});
```  

策略冲突错误会将`DiscoveryResponse`上的`ErrorType`属性设置为`PolicyViolation`。

## 1.2 缓存发现文档
您应该定期更新发现文档的本地副本，以便能够对服务器上的配置更改作出响应。这对于使用自动旋转键进行良好的播放尤其重要。

该`DiscoveryCache`类可以帮助你。

以下代码将设置缓存，在第一次需要时检索文档，然后将其缓存24小时：

``` C#
var cache = new DiscoveryCache("https://demo.identityserver.io");
```   

然后，您可以像这样访问文档：

``` C#
var disco = await cache.GetAsync();
if (disco.IsError) throw new Exception(disco.Error);
```   

您可以使用该`CacheDuration`属性指定缓存持续时间，也可以通过将`DiscoveryPolicy`传递给构造函数来指定自定义发现策略。

### 1.2.1 缓存和HttpClient实例
默认情况下，发现缓存将在`HttpClient`每次访问发现端点时创建新实例。您可以通过两种方式修改此行为，方法是将预先创建的实例传入构造函数，或者通过提供将`HttpClient`在需要时返回的函数。

以下代码将在DI中设置发现缓存，并将使用`HttpClientFactory`以创建客户端：

``` C#
services.AddSingleton<IDiscoveryCache>(r =>
{
    var factory = r.GetRequiredService<IHttpClientFactory>();
    return new DiscoveryCache(Constants.Authority, () => factory.CreateClient());
});
```