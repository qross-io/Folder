# 在 PQL 中获取 URL 地址及其他请求信息

在接口开发逻辑中，有一个变量`$request`可以获取当前页面或网址请求信息。这个变量是一个数据行类型，包含以下字段：

* `method` HTTP 的请求方法
* `url` 完整的地址
* `protocol` 协议类型，是`HTTP`还是`HTTPS`
* `domain` 主机名或域名，不含端口
* `host` 同`domain`
* `port` 端口号
* `path` 域名和端口号后面的路径
* `queryString` 地址中的查询字符串，即问号`?`后面的内容

如下地址
```
http://localhost:8080/api/example/test?id=1&score=89
```

* `url`为`http://localhost:8080/api/example/test?id=1&score=89`
* `protocol`为`HTTP`
* `domain`和`host`为`localhost`
* `prot`为`8080`
* `path`为`/api/example/test`
* `queryString`为`id=1&score=89`

---
参考链接

* [向 OneApi 的接口传递参数](/oneapi/params.md)