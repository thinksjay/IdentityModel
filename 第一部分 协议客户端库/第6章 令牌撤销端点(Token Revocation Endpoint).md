# 第6章 令牌撤销端点(Token Revocation Endpoint)
OAuth 2.0令牌撤销的客户端库是作为扩展方法提供的HttpClient。

以下代码撤消撤销端点处的访问令牌令牌：

``` C#
var client = new HttpClient();

var result = await client.RevokeTokenAsync(new TokenRevocationRequest
{
    Address = "https://demo.identityserver.io/connect/revocation",
    ClientId = "client",
    ClientSecret = "secret",

    Token = accessToken
});
```   

响应属于`TokenRevocationResponse`类型使您可以访问原始响应以及解析的JSON文档（通过`Raw`和`Json`属性）。

在使用响应之前，您应该始终检查`IsError`属性以确保请求成功：

``` C#
if (response.IsError) throw new Exception(response.Error);
```