# 第3章 结束会话端点(EndSession Point)
该[RequestUrl](https://github.com/thinksjay/IdentityModel/blob/master/%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86%20%E6%9D%82%E9%A1%B9%E5%8A%A9%E6%89%8B/%E7%AC%AC11%E7%AB%A0%20%E5%88%9B%E5%BB%BA%E8%AF%B7%E6%B1%82URLs.md)类可用于构造URL发送到OpenID Connect EndSession endpoint。

该`CreateEndSessionUrl`扩展方法支持最常用的参数：

``` C#
/// <summary>
/// Creates a end_session URL.
/// </summary>
/// <param name="request">The request.</param>
/// <param name="idTokenHint">The id_token hint.</param>
/// <param name="postLogoutRedirectUri">The post logout redirect URI.</param>
/// <param name="state">The state.</param>
/// <param name="extra">The extra parameters.</param>
/// <returns></returns>
public static string CreateEndSessionUrl(this RequestUrl request,
    string idTokenHint = null,
    string postLogoutRedirectUri = null,
    string state = null,
    object extra = null)
{ ... }
```   

> **注意**
该`extra`参数可以是一个串字典或任意其它类型的具有属性。在这两种情况下，值都将序列化为键/值。