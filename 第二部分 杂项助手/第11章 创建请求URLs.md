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

`RequestUrl`在创建用于建模特定于域的URL结构的扩展方法时，它非常有用。有关示例，请参阅[Authorize Endpoint]()和[EndSession Endpoint]()。