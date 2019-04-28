# 第11章 创建请求URLs
该`RequestUrl`是创建与查询字符串参数，例如URL的帮手：

``` C#
var ru = new RequestUrl("https://server/endpoint");

// produces https://server/endpoint?foo=foo&bar=bar
var url = ru.Create(new
    {
        foo: "foo",
        bar: "bar"
    });
```   

作为`Create`方法的参数，您可以传入对象或字符串字典。在这两种情况下，属性/值都将序列化为键/值对。

> **注意**
所有值都将进行URL编码。

`RequestUrl`在创建用于建模特定于域的URL结构的扩展方法时，它非常有用。有关示例，请参阅[Authorize Endpoint](https://github.com/thinksjay/IdentityModel/blob/master/%E7%AC%AC%E4%B8%80%E9%83%A8%E5%88%86%20%E5%8D%8F%E8%AE%AE%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BA%93/%E7%AC%AC2%E7%AB%A0%20%E6%8E%88%E6%9D%83%E7%AB%AF%E7%82%B9(Authorize%20Endpoint).md)和[EndSession Endpoint]()。