# 第5章 令牌自省端点(Token Introspection Endpoint)
[OAuth 2.0令牌自省](https://tools.ietf.org/html/rfc7662)的客户端库是作为`HttpClient`扩展方法提供的。

以下代码将引用令牌发送到内省端点：

``` C#
var client = new HttpClient();

var response = await client.IntrospectTokenAsync(new TokenIntrospectionRequest
{
    Address = "https://demo.identityserver.io/connect/introspect",
    ClientId = "api1",
    ClientSecret = "secret",

    Token = accessToken
});
```  

响应属于`IntrospectionResponse`类型并具有标准响应参数的属性。您还可以访问原始响应以及解析的JSON文档（通过`Raw`和`Json`属性）。

在使用响应之前，您应该始终检查`IsError`属性以确保请求成功：

``` C#
if (response.IsError) throw new Exception(response.Error);

var isActive = response.IsActive;
var claims = response.Claims;
```