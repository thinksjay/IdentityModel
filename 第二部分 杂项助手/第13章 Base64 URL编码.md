# 第13章 Base64 URL编码
JWT令牌使用Base64 URL编码进行序列化。

IdentityModel包括`Base64Url`帮助编码/解码的类：

``` C#
var text = "hello";
var b64url = Base64Url.Encode(text);

text = Base64Url.Decode(b64url);
```  

> **注意**
ASP.NET Core通过[WebEncoders.Base64UrlEncode](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.webencoders.base64urlencode)和[WebEncoders.Base64UrlDecode](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.webencoders.base64urldecode)提供内置支持。