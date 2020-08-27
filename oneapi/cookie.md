# 在OneApi中使用Cookie和Session

### 获取Cookie和Session

在Spring Boot环境下，PQL提供了两个全局变量来读取Cookie和Session，分别为`@COOKIES`和`@SESSION`。直接使用Cookie和Session的名字访问对应的值。例如：
```sql
IF @COOKIES.language == 'english' THEN ...

PRINT @SESSION.username;
```

如果Cookie或Session的名字输入错误或找不到这个名字，将返回`UNDEFINED`。

### 更新Cookie和Session

暂时不支持，将在新版本中加入。


---
参考链接

* [PQL全局变量](/pql/global.md)
* [向OneApi的接口传递参数](/oneapi/params.md)
* [在OneApi中获取URL地址等信息](/oneapi/request.md)