# 在 OneApi 中使用 Cookie 和 Session

## 获取 Cookie 和 Session

在 Spring Boot 环境下，PQL 提供了两个全局变量来读取 Cookie 和 Session，分别为`@COOKIES`和`@SESSION`。直接使用 Cookie 和 Session 的名字访问对应的值。例如：

```sql
IF @COOKIES.language == 'english' THEN ...

PRINT @SESSION.username;
```

如果 Cookie 或 Session 的名字输入错误或找不到这个名字，将返回`UNDEFINED`。

## 更新 Cookie 和 Session

暂时不支持，将在未来版本中加入。

---
参考链接

* [PQL 全局变量](/pql/global-variable.md)
* [向 OneApi 的接口传递参数](/oneapi/params.md)
* [在 OneApi 中获取 URL 地址等信息](/oneapi/request.md)