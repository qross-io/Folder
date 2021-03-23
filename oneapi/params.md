# 向 OneApi 的接口传递参数

OneApi 构建的接口基于 HTTP 协议访问，所以向 OneApi 传参也使用 HTTP 的方式，OneApi 支持两种方式的传参，一种是 URL 中的查询字符串方式，二是 Json 对象方式。

## 使用查询字符串向接口传递参数

```
http://localhost:8080/api/exapmle/test?id=1&score=89
```

上面接口地址中问号之后的部分`id=1&score=89`即是传递的参数，OneApi 会把这些参数解析成键值对，传给接口逻辑使用。

OneApi 安全验证使用的请求者名称和 Token 需要通过这种方式传递。如

```
http://localhost:8080/api/exapmle/test?id=1&score=89&reuqester=teacher&token=npeiwxl
```

## 向 OneApi 接口传递 Json 对象

OneApi 的每个接口还可以接收 Json 对象类型的数据，OneApi 同样会把 Json 对象解析成键值对，传递给接口逻辑使用。举个例子：

```sql
REQUEST JSON API 'http://localhost:8080/api/exapmle/test'
    USE METHOD "PUT"
    SEND DATA { "id": 1, "score": 89 };
```

## 在接口逻辑中使用参数

因为接口逻辑使用 PQL 过程实现，所以 OneApi 处理参数的过程就是 PQL 处理参数的过程。请参照 [PQL 参数处理](/pql/params.md)。如果想在 PQL 中使用参数或 URL 地址中的信息，请参阅[在 PQL 中获取 URL 地址及其他请求信息](/oneapi/request.md)

---
参考链接

* [创建和编辑接口](/oneapi/edit.md)
* [OneApi 安全验证](/oneapi/token.md)
* [PQL 处理参数](/pql/param.md)
* [在 OneApi 中获取URL地址等信息](/oneapi/request.md)
* [请求接口 REQUEST](/pql/request.md)