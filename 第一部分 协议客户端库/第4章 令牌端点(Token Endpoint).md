# 第4章 令牌端点(Token Endpoint)
令牌端点的客户端库（[OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-3.2)和[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html#TokenEndpoint)）作为`HttpClient`一组扩展方法提供。这允许`HttpClient`以您喜欢的方式创建和管理生命周期- 例如静态或通过像Microsoft这样的工厂`HttpClientFactory`。

## 4.1 请求令牌
调用主扩展方法`RequestTokenAsync`- 它直接支持标准参数，如客户端ID /机密（或断言）和授权类型，但它也允许通过字典设置任意其他参数。所有其他扩展方法最终在内部调用此方法：

``` C#
var client = new HttpClient();

var response = await client.RequestTokenAsync(new TokenRequest
{
    Address = "https://demo.identityserver.io/connect/token",
    GrantType = "custom",

    ClientId = "client",
    ClientSecret = "secret",

    Parameters =
    {
        { "custom_parameter", "custom value"},
        { "scope", "api1" }
    }
});
```   

响应属于`TokenResponse`类型并且具有用于标准令牌响应参数等属性`access_token`，`expires_in`等等。你也可以访问原始响应以及对已解析JSON的文档（通过`Raw`和`Json`属性）。

在使用响应之前，您应该始终检查`IsError`属性以确保请求成功：

``` C#
if (response.IsError) throw new Exception(response.Error);

var token = response.AccessToken;
var custom = response.Json.TryGetString("custom_parameter");
```  

## 4.2 使用`client_credentials`授权类型请求令牌
该方法具有方便`requestclientcredentialstoken`扩展属性的`client_credentials`类型：

``` C#
var response = await client.RequestClientCredentialsTokenAsync(new ClientCredentialsTokenRequest
{
    Address = "https://demo.identityserver.io/connect/token",

    ClientId = "client",
    ClientSecret = "secret",
    Scope = "api1"
});
```   

## 4.3 使用`password`授权类型请求令牌
该方法具有方便`requestclientcredentialstoken`扩展属性的`password`类型：

``` C#
var response = await client.RequestPasswordTokenAsync(new PasswordTokenRequest
{
    Address = "https://demo.identityserver.io/connect/token",

    ClientId = "client",
    ClientSecret = "secret",
    Scope = "api1",

    UserName = "bob",
    Password = "bob"
});
```   

## 4.4 使用`authorization_code`授权类型请求令牌
该方法具有方便`requestclientcredentialstoken`扩展属性的`authorization_code`类型和PKCE：

``` C#
var response = await client.RequestAuthorizationCodeTokenAsync(new AuthorizationCodeTokenRequest
{
    Address = IdentityServerPipeline.TokenEndpoint,

    ClientId = "client",
    ClientSecret = "secret",

    Code = code,
    RedirectUri = "https://app.com/callback",

    // optional PKCE parameter
    CodeVerifier = "xyz"
});
```   

## 4.5 使用`refresh_token`授权类型请求令牌
该方法具有方便`requestclientcredentialstoken`扩展属性的`refresh_token`类型：

``` C#
var response = await _client.RequestRefreshTokenAsync(new RefreshTokenRequest
{
    Address = TokenEndpoint,

    ClientId = "client",
    ClientSecret = "secret",

    RefreshToken = "xyz"
});
```   

## 4.6 请求设备令牌
该方法具有方便`requestclientcredentialstoken`扩展属性的`urn:ietf:params:oauth:grant-type:device_code`类型

``` C#
var response = await client.RequestDeviceTokenAsync(new DeviceTokenRequest
{
    Address = disco.TokenEndpoint,

    ClientId = "device",
    DeviceCode = authorizeResponse.DeviceCode
});
```