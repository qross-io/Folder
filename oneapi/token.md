# OneApi 基于Token访问安全验证

OneApi安全控制主要通过 **Token** 来实现，Token分为两种，第一种是静态Token，第二种是动态Token。相对于来说，动态Token更安全一些。另外一种通过登录用户来控制权限，详见[OneApi登录用户](/oneapi/signin.md)。

基于文件的接口管理方式中，Token在配置文件中设置，且Token的应用范围仅限当前项目; 基于数据库的接口管理方式中，Token在数据为中设置，应用范围是由数据库管理的所有项目。

### 使用静态Token
当设置项`oneapi.security.option`的值为`token`时，启用Token验证模式。静态Token类似于普通用户登录的的用户名和密码，需要在请求接口时提供额外的参数`token`。例如
```
http://localhost:8080/api/example/test?token=npeiwxl
```

基于文件管理的接口方式Token在设置项`oneapi.token.list`中设置，参见[OneApi设置](/oneapi/setup.md)。`oneapi.token.list`设置项的格式为`requester1=token1&requester2=token2&requester3=token3`，字符与字符之间不能有空格，且token必须唯一，即不同请求者的Token也不能一样。例如`teacher=npeiwxl&monitor=xequtnb`。

### 使用动态Token

当设置项`oneapi.security.option`的值为`secret`时，启用动态Token验证模式。动态Token需要先请求一个接口获取动态Token，再用这个动态Token请求需要的接口，需要两步操作。但是动态Token有过期时间，在过期时间之内，可以不用再次获取动态Token。

动态Token的获取接口示例如下，返回值是一个字符串：
```
http://localhost:8080/oneapi/secret?token=npeiwxl
```
这个接口需要自己实现，调用方法`OneApi.getSecretKey(String token)`即可，示例如下：
```java
@RequestMapping("/oneapi/secret")
public String OneApiSecret(@RequestParam(value = "token") String token) {
    return OneApi.getSecretKey(token);
}
```
另一个方法可以获取动态Token的剩余存活时间（time to live） `OneApi.ttlSecretKey(String key)`。

请求目标接口的示例如下，参数`secret`即动态Token：
```
http://localhost:8080/api/example/test?secret=xnjmiqt
```

建议使用Redis保存动态Token，特别是在负载均衡模式下，只需在配置文件中配置名称为`qross`的Redis数据源连接即可，可参见[PQL数据源配置](/pql/properties.md)。例如
```s
redis.qross.host=localhost
redis.qross.port=6379
redis.qross.password=
redis.qross.database=0
```
**需要在项目中引入依赖`jedis 3.0`或以上版本**

与动态Token相关的设置项有两个：
* `oneapi.secret.key.ttl` 可以设置动态Token的过期时间，默认时间为`3600`秒。
* `oneapi.secret.key.digit` 动态Token的位数，默认值是`16`位。


请求接口时OneApi会验证请求者名称和Token是否匹配，判断请求者能不能访问这个服务，但是请求者是否有访问此接口的权限，还需要进行安全控制。就像用户系统一样，用户登录了系统，但是用户能够使用哪些功能还需要设置相应的权限。详见[OneApi接口安全控制](/oneapi/permit.md)。

在接口服务中，有的接口可能是完全开放的，不需要安全验证也能够访问，OneApi可以设置不需要安全验证也够访问的接口，详见[OneApi设置安全验证例外](/oneapi/open.md)。

---
参考链接

* [OneApi全局设置](/oneapi/setup.md)
* [OneApi登录用户](/oneapi/signin.md)
* [PQL数据源配置](/pql/properties.md)
* [OneApi接口安全控制](/oneapi/permit.md)
* [OneApi设置安全验证例外](/oneapi/open.md)