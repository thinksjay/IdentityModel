# 第9章 设备授权端点(Device Authorization Endpoint)
[OAuth 2.0设备流](https://tools.ietf.org/html/rfc7662)设备授权的客户端库是作为`HttpClient`扩展方法提供的。

以下代码发送设备授权请求：

``` C#
var client = new HttpClient();

var response = await client.RequestDeviceAuthorizationAsync(new DeviceAuthorizationRequest
{
    Address = "https://demo.identityserver.io/connect/device_authorize",
    ClientId = "device"
});
```   

响应属于`DeviceAuthorizationResponse`类型并具有标准响应参数的属性。您还可以访问原始响应以及解析的JSON文档（通过`Raw`和`Json`属性）。

在使用响应之前，您应该始终检查IsError属性以确保请求成功：

``` C#
if (response.IsError) throw new Exception(response.Error);

var userCode = response.UserCode;
var deviceCode = response.DeviceCode;
var verificationUrl = response.VerificationUri;
var verificationUrlComplete = response.VerificationUriComplete;
```