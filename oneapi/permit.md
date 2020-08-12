# OneApi 访问安全控制

在配置好安全验证之后，还需要配置请求者有权限访问哪些接口，就和用户系统需要配置用户权限一样。通过设置项`oneapi.access.permit`进行设置。
```
oneapi.access.permit=*=*
oneapi.access.permit=/api/*=*
oneapi.access.permit=/api/example/*=tom;GET:/api/*=jerry
oneapi.access.permit=/api/example/test=tom,jerry;GET,POST:/api/score/*=tom
```

* 每一项控制规则由等号`=`连接的左右两部分，等号左边指路径，右边是请求者。
* 留空和`*=*`都表示允许全部访问。前一个`*`表示全部接口，后一个星表示全部请求者。
* `/api/*=*`表示允许所有请求者访问`/api/`下的所有接口。
* `/api/example/*=tom`表示允许`tom`访问`/api/example/`下的所有接口。
* `GET:/api/*=jerry`表示允许`jerry`访问`/api/`目录下请求方法为`GET`的所有接口。
* `/api/example/test=tom,jerry`表示`tom`和`jerry`可以访问接口`/api/example/test`。
* `GET,POST:/api/score/*=tom`表示`tom`可以使用请求方法`GET`或`POST`访问`/api/score/`目录下的所有接口。
* 上述规则没有指定请求方法的情况下，都是允许所有请求方法。
* 单个接口的权限控制一般设置在接口定义上，在接口上的定义控制的是 **请求方法+接口**。

访问安全控制同时适用于基于登录用户的访问控制，与Token不同，在控制规则的等号`=`右侧，可以设置用户的角色，角色以`@`开头。即以`@`开头的是角色，否则是用户。
```
oneapi.access.permit=/api/*=sam,@teacher
```
* `/api/*=sam,@teacher`表示把用户`sam`和角色`@teacher`可以访问`/api/`目录下的所有接口。


---
参考链接
* [OneApi全局设置](/doc/oneapi/setup)
* [OneApi用户登录](/doc/oneapi/signin)
* [OneApi设置安全验证例外](/doc/oneapi/open)