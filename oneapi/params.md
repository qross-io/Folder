# 向OneApi传递参数
OneApi构建的接口基于HTTP协议访问，所以向OneApi传参也使用HTTP的方式，OneApi支持两种方式的传参，一种是URL中的查询字符串方式，二是Json对象方式。

#### 使用查询字符串向接口传递参数
```
http://localhost:8080/api/exapmle/test?id=1&score=89
```
上面接口地址中问号之后的部分`id=1&score=89`即是传递的参数，OneApi会把这些参数解析成键值对，传给接口逻辑使用。

OneApi安全验证使用的请求者名称和Token需要通过这种方式传递。如
```
http://localhost:8080/api/exapmle/test?id=1&score=89&reuqester=teacher&token=npeiwxl
```

#### 向OneApi接口传递Json对象
OneApi的每个接口还可以接收Json对象类型的数据，OneApi同样会把Json对象解析成键值对，传递给接口逻辑使用。举个例子：
```sql
REQUEST JSON API 'http://localhost:8080/api/exapmle/test'
    USE METHOD "PUT"
    SEND DATA { "id": 1, "score": 89 };
```

#### 在接口逻辑中使用参数
因为接口逻辑使用PQL过程实现，所以OneApi处理参数的过程就是PQL处理参数的过程。请参照[PQL参数处理](/doc/pql/params)。

---
参考链接
* [创建和编辑接口](/doc/oneapi/edit)
* [OneApi安全验证](/doc/oneapi/token)
* [PQL处理参数](/doc/pql/param)
* [请求接口 REQUEST](/doc/pql/request)