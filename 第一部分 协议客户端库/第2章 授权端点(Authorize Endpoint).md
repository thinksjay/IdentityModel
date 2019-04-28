# 第2章 授权端点(Authorize Endpoint)
对于大多数情况，[OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-3.1)和[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint)授权端点的GET请求需要具有许多查询字符串参数。

虽然您可以使用任何方法创建带参数的URL来创建正确的字符串，但[RequestUrl](https://github.com/thinksjay/IdentityModel/blob/master/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%20%E6%9D%82%E9%A1%B9%E5%8A%A9%E6%89%8B/%E7%AC%AC11%E7%AB%A0%20%E5%88%9B%E5%BB%BA%E8%AF%B7%E6%B1%82URLs.md)类是完成此任务的简单帮助程序。

特别是，您可以使用`CreateAuthorizeUrl`扩展方法为授权端点创建URL - 它支持最常用的参数：

``` C#
/// <summary>
/// Creates an authorize URL.
/// </summary>
/// <param name="request">The request.</param>
/// <param name="clientId">The client identifier.</param>
/// <param name="responseType">The response type.</param>
/// <param name="scope">The scope.</param>
/// <param name="redirectUri">The redirect URI.</param>
/// <param name="state">The state.</param>
/// <param name="nonce">The nonce.</param>
/// <param name="loginHint">The login hint.</param>
/// <param name="acrValues">The acr values.</param>
/// <param name="prompt">The prompt.</param>
/// <param name="responseMode">The response mode.</param>
/// <param name="codeChallenge">The code challenge.</param>
/// <param name="codeChallengeMethod">The code challenge method.</param>
/// <param name="display">The display option.</param>
/// <param name="maxAge">The max age.</param>
/// <param name="uiLocales">The ui locales.</param>
/// <param name="idTokenHint">The id_token hint.</param>
/// <param name="extra">Extra parameters.</param>
/// <returns></returns>
public static string CreateAuthorizeUrl(this RequestUrl request,
    string clientId,
    string responseType,
    string scope = null,
    string redirectUri = null,
    string state = null,
    string nonce = null,
    string loginHint = null,
    string acrValues = null,
    string prompt = null,
    string responseMode = null,
    string codeChallenge = null,
    string codeChallengeMethod = null,
    string display = null,
    int? maxAge = null,
    string uiLocales = null,
    string idTokenHint = null,
    object extra = null)
{ ... }
```   

例：

``` C#
var ru = new RequestUrl("https://demo.identityserver.io/connect/authorize");

var url = ru.CreateAuthorizeUrl(
    clientId: "client",
    responseType: "implicit",
    redirectUri: "https://app.com/callback",
    nonce: "xyz",
    scope: "openid");

```   

> **注意**
该`extra`参数可以是一个串字典或任意其它具有属性的类型。在这两种情况下，值都将序列化为键/值。