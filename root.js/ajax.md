# Ajax 请求全局设置

在前端开发中，请求接口是与后端交互最经常的操作。如下是一个典型的接口格式：

```json
[
    {
        "name": "Tom",
        "age": 19
    },
    {
        "name": "Jerry",
        "age": 18
    }
]
```

当接口正常时，返回以上数据。当接口错误时，则调用会失败，返回 500 错误。但是很多情况下，接口的根节点并不是所需要的数据，比如下面是一种更常见的接口格式：

```json
{
    timestamp: "2021-04-07 22:46:05",
    elapsed: 55,
    status: "success",
    message: "",    
    data: [
        {
            "name": "Tom",
            "age": 19
        },
        {
            "name": "Jerry",
            "age": 18
        }
    ]
}
```

上例中：

* `timestamp` 接口请求时间戳。
* `elapsed` 后端逻辑的处理时间，单位是毫秒。
* `status` 当接口执行过程中不发生错误时，返回`success`，否则返回其他出错信息。当`status`为`success`时，这个接口的数据才可以用。
* `message` 当接口出错时这里会显示错误信息。
* `data` 当接口正常时，这里是前端所需要的数据，否则为`null`。

这种情况下，在使用数据之前我们就要对接口返回的数据先进行各种判断，例如：

```javascript
//省略请求的部分
.then (result => {
    if (result.status == 1) {
        //使用数据 result.data
    }
    else {
        console.error(result.message);
    }
});
```

如果把以上过程写在每个接口处理逻辑中，会是一个比较烦琐和痛苦的操作。在 root.js 中，提供了一个全局设置，保存在 Cookie 中，Cookie 的名字为`oneapi.ajax.settings`，Cookie 的值是一个 Json 字符串，格式为：

```json
[
    {
        "pattern": "/api/", 
        "ready": "status == 'success'", 
        "info": "/message",
        "path": "/data"
    }
]
```

以上各参数的解释如下：

* `pattern` 匹配一个或多个接口，上例中，所有包含`/api/`的接口都应用这个规则，支持正则表达式匹配（不区分大小写）。
* `ready` 接口可用的条件，如上例是`status == 'success'`。
* `info` 当接口异常时，指明从哪里获取错误信息，并发送给 Promise 对象的`reject`方法，JsonPath 格式。
* `path` 当接口正常时，指明从哪里获取数据，并发送给 Promise 对象的`resolve`方法，JsonPath 格式。

设置好后，再使用`/api/`相关的接口时，就不需要每次都写上面的判断逻辑了。可以设置多个匹配规则，以针对不同的接口格式。root.js 中提供了 Cookie 设置的方法：

```javascript
$cookie.set('oneapi.ajax.settings', '[{"pattern":"/api/",...}]');
```

这个设置也可以[通过 OneApi 进行设置](/oneapi/setup.md)。

---
参考链接

* [与数据相同的属性](/root.js/data.md)
* [服务器端事件](/root.js/server.md)
* [OneApi 设置](/oneapi/setup.md)