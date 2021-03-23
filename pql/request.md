# 请求 Json 数据接口 REQUEST 语句

无论是在数据开发中还是后端开发中，请求其他服务的接口是经常的操作。PQL 提供了请求接口的语句 REQUEST 及解析接口内容的语句 PARSE 用来解决开发过程中的接口访问问题。

```sql
REQUEST JSON API 'http://domain.com/api?id=1'
    USE METHOD "POST"
    SEND DATA {"data": "value"}
    SET HEADER {"name": "value"};
```

* 仅支持 Json 接口的请求。
* 请求地址可以是绝对地址也可以是相对地址。
* 查询参数都放在地址后面，无论使用哪个 METHOD。
* `USE METHOD`设置请求接口的 METHOD，可选`GET`, `POST`,`PUT`, `DELETE`，`USE`关键词可省略。
* `SEND DATA`用来传递 Json 格式的参数，只支持数据行（对象）格式，`SEND`关键词可省略。
* `SET HEADER`用来设置 Http 头，支持数据行（对象）格式和查询字符串格式（如 name=Tom&age=18&score=89），`SET`关键词可省略。

REQUEST 语句负责请求接口并把返回的数据保存在缓冲区当中，剩下的工作交给 PARSE 语句。保存在缓冲区的数据直到下一次请求接口才会清除。

---
参考链接

* [解析 Json 接口返回的数据 PARSE](/pql/parse.md)