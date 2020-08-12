# OneApi类

这里汇总一下OneApi类中的方法，方便查阅。
```java
public static boolean contains(String path);
public static boolean contains(String path, String method);
public static Map<String, String> getAll();
public static String getToken(int digit);
public static OneApi pick(String path, String method);
public static int refresh();
public static OneApiRequester signIn(int userId, String userName, String role);
public static OneApiRequester signIn(Map<String, Object> info);
public static OneApiRequester signIn(int userId, String userName, String role, Map<String, Object> info);
public static Object request(String path);
public static Object request(String path, Map<String, String> params);
```

* `contains`方法用来判断某个接口是否已经定义
* `getAll` 方法返回把有接口
* `pick` 根据路径和方法得到某个接口的详细内容
* `refresh` 重新加载所有接口，更新后使用
* `signIn` 将用户登录信息传递给OneApi
* `request` 请求某个接口，可以自定义参数

在自定义的`@RestController`类中，只需要调用`signIn`和`request`方法即可，其他均为管理方法，见[OneApi管理功能](/doc/oneapi/management)。调用示例：
```java
OneApi.request(path);
OneApi.signIn(1, username, role).request(path);
```

---
参考链接
* [OneApi快速入门](/doc/oneapi/quick)
* [OneApi用户登录](/doc/oneapi/signin)
* [OneApi管理功能](/doc/oneapi/management)