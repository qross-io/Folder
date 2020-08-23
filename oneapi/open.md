# 设置对所有请求者开放的接口

在OneApi服务中，有时不是所有的接口都需要安全验证，可以用[全局设置](/oneapi/setup.md)项`oneapi.access.open`来设置哪些接口不需要安全验证。例如：
```s
oneapi.access.open=
oneapi.access.open=*
oneapi.access.open=/api/*;/api/example/test
oneapi.access.open=GET:/api/*;GET,DELETE:/api/example/*
```
分别解释一下各个值代表的意义
* 设置为空表示所有接口都需要安全验证。
* `*`代表所有接口，即所有接口都不需要安全验证，一般不会这么做。
* `/api/*`代表开放`/api/`目录下的所有接口。
* `/api/example/test`表示单独开放这个接口。
* `GET:/api/*`代表开放`/api/`目录下所有请求方法为`GET`的接口。
* `GET,DELETE:/api/example/*`表示开放`/api/example/`目录下所有请求方法为`GET`或`DELETE`的接口，多个请求方法之间使用逗号`,`分隔。
* 多个规则之间使用分号`;`隔开，字符和字符之间不能有空格。
* 规则验证模式是单个接口优先匹配，子目录优先匹配。


设置每个请求者的接口访问权限，逻辑类似，请参照[OneApi接口安全控制](/oneapi/permit.md)。

---
参考链接
* [OneApi全局设置](/oneapi/setup.md)
* [OneApi访问安全验证](/oneapi/token.md)
* [OneApi访问安全控制](/oneapi/permit.md)