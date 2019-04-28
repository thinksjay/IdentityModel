# 第7章 UserInfo端点(UserInfo Endpoint)
OpenID Connect UserInfo端点的客户端库是作为扩展`HttpClient`方法提供的。

以下代码将访问令牌发送到UserInfo端点：

``` C#
var client = new HttpClient();

var response = await client.GetUserInfoAsync(new UserInfoRequest
{
    Address = disco.UserInfoEndpoint,
    Token = token
});
```  

响应属于`UserInfoResponse`类型并具有标准响应参数的属性。您还可以访问原始响应以及解析的JSON文档（通过`Raw`和`Json`属性）。

在使用响应之前，您应该始终检查IsError属性以确保请求成功：

``` C#
if (response.IsError) throw new Exception(response.Error);

var claims = response.Claims;
```