# OneApi 设置
OneApi使用`conf.properties`文件保存全局设置，`conf.properties`必须放在项目的`resources`目录下。OneApi的所有相关设置项如下：
```
oneapi.service.name=example
oneapi.management.key=1234567
oneapi.resources.dirs=/api/
oneapi.security.mode=none
oneapi.security.option=3600
oneapi.token.list=name1=token1&name2=token2&name3=token3
oneapi.access.open=
oneapi.access.permit=*=*
```

* `onapi.service.name` 如果使用基于数据库的接口管理方式，项目启动时根据这个名字去数据库加载相应的接口，另外有些管理功能也需要这个名称，名称需唯一。如果仅使用基于文件的方式管理接口，不要设置此项。
* `oneapi.management.key` OneApi服务管理密码，OneApi服务提供了一些管理功能，如刷新接口，修改设置等。使用这些管理功能需要设置管理密码，可通过[OneApi类](/doc/oneapi/classS)中的`OneApi.getToken(int digit)`方法生成随机密码。管理功能详见 [OneApi接口管理](/doc/oneapi/management)
* `oneapi.resources.dirs` 表示保存在`resources`目录下的接口文件的目录，可以设置多个目录。如果设置多个，目录名称之间使用逗号分隔。OneApi根据这个设置扫描这些目录下的所有文件，用户请求时再调用相应的接口。详见 [用目录组织接口文件](/doc/oneapi/file)。
* `oneapi.security.mode` 接口访问安全控制，特别是接口开放到外网时尤其重要。可选`none`，无安全控制; `token` 静态Token认证，为每一个接口请求者分配一个用户名和Token，请求者每次访问接口必须携带这两个参数，匹配上才能够继续访问接口; `securet` 动态Token认证，每次接口请求前必须先请求一次Token获取接口，获取动态Token，然后再用动态Token继续请求接口; `user` 使用用户登录信息做安全控制，不做安全验证，只做接口的访问控制。详见 [OneApi访问安全控制](/doc/oneapi/token)。
* `oneapi.security.option` 动态Token模式下，这个值用来设置Token多长时间过期，单位秒。
* `oneapi.token.list` OneApi的请求者和Token列表，请求者可以是其他业务部门、或者其他微服务、或其他模块的名字，Token建议使用`OneApi.getToken(int digit)`方法生成，详见[OneApi类](/doc/oneapi/class)。Token相关逻辑详见 [OneApi访问安全验证](/doc/oneapi/token)。
* `oneapi.access.open` 不需要安全验证也能够访问的接口地址各地址规则，详见 [OneApi设置安全验证例外](/doc/oneapi/open)。
* `oneapi.access.permit` 配置需要认证接口的访问权限，即使有安全控制，每个接口也不是每个请求者都可以访问，比如有的请求者只能访问服务中的几个接口。这个参数可以对访问进行批量控制，详见 [OneApi访问安全控制](/doc/oneapi/permit)。

---
参考链接
* [数据库连接设置](/doc/pql/properties)
* [在Master中管理所有数据接口](/doc/master/oneapi)
* [OneApi管理功能](/doc/oneapi/management)
* [用目录组织接口文件](/doc/oneapi/file)
* [OneApi访问安全验证](/doc/oneapi/token)
* [OneApi访问安全控制](/doc/oneapi/permit)
* [OneApi类](/doc/oneapi/class)