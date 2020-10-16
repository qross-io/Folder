# OneApi类

OneApi类所在的包为`io.qross.app.OneApi`。这里汇总一下OneApi类中的方法，方便查阅。
```java
public static boolean OneApi.authenticateManagementKey(key);
public static boolean contains(String path);
public static boolean contains(String path, String method);
public static int count();
public static Map<String, String> getAll();
public static Map<String, String> get(String path);
public static String get(String path, String method);
public static String getSecretKey(String token);
public static String getSetting(String key);
public static Map<String, Object> getSettings();
public static String getToken(int digit);
public static int refresh();
public static OneApiRequester signIn(int userId, String userName, String role);
public static OneApiRequester signIn(Map<String, Object> info);
public static OneApiRequester signIn(int userId, String userName, String role, Map<String, Object> info);
public static Object request(String path);
public static Object request(String path, Map<String, String> params);
```

* `authenticateManagementKey`方法用来验证指定的管理密钥是否正确。
* `contains`方法用来判断某个接口是否已经定义。
* `count`方法返回所有接口数量。
* `get`方法返回接口内容，不含Method参数的`get`方法返回Methd和对应接口内容的HashMap。
* `getAll` 方法返回所有接口，返回接口路径和Method的HashMap，不含接口逻辑。
* `getSecretKey`方法用来根据`token`获取动态Token，详见[使用动态Token](/oneapi/token.md)。
* `getToken`方法用于生成随机`digit`位数的字符串，字符串中只会包含大小写字母和数字，如`getToken(10)`可能得到`N7ax8E9piM`。
* `getSetting`方法用来查看某个设置项的值。
* `getSettings`方法返回所有设置项及对应的值。
* `refresh` 重新加载所有接口，更新接口后使用，返回接口路径的数量（多个Method算1个）。
* `signIn` 将用户登录信息传递给OneApi。
* `request` 请求某个接口，可以自定义参数。

在自定义的`@RestController`类中，只需要调用`signIn`和`request`方法即可，其他均为管理方法，见[OneApi管理功能](/oneapi/management.md)。调用示例：
```java
OneApi.request(path);
OneApi.signIn(1, username, role).request(path);
```

---
参考链接

* [OneApi快速入门](/oneapi/quick.md)
* [OneApi登录用户](/oneapi/signin.md)
* [OneApi管理功能](/oneapi/management.md)