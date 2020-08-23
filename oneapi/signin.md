# OneApi登录用户

OneApi中的用户登录更确切应该是说把登录信息传给OneApi。因为各用户系统的千差万别，所以OneApi无法做到登录用户鉴权，本身也没有必要，使用者可以在使用`OneApi.request()`方法之前完成这项工作。这里中介绍一下如何把用户信息传递给OneApi，以及如何在接口逻辑中使用这些信息。

### 使用OneApi的signIn方法传递用户登录信息
OneApi类提供了一个`signIn`方法来接收用户的登录信息，并提供了几个重载，可以在`request`方法之前调用。
```java
public static OneApiRequester signIn(int userId, String userName, String role);
public static OneApiRequester signIn(Map<String, Object> info);
public static OneApiRequester signIn(int userId, String userName, String role, Map<String, Object> info);
```
应用示例：
```java
OneApi.signIn(1, 'Tom', 'student').request('/api/example/test?id=1');
OneApi.signIn(UserWebAuthenticationDetails.getCredential()).request(path);
```
上例第一个示例，是把用户的三项基本信息传递给OneApi。第二个示例是直接把 **Sping Security** 的鉴权信息直接传给OneApi。

特别注意：使用第二个示例的重载传递登录信息时，必须包含`userid`、`username`和`role`这三个属性，否则在PQL中无法获取，安全控制也无法工作。

### 在OneApi的接口逻辑中使用用户登录信息
OneApi会把用户登录信息保存下来，保存在全局变量中，可以在PQL逻辑中通过全局变量直接引用这些信息。假设登录信息的结构是这样的
```json
{
    "userid": 2,
    "usrname": "sam",
    "role": "monitor",
    "email": "sam@school.com",
    "mobile": "42883256"
}
```
那么就有以下全局变量可用
* `@userid` 值为 2
* `@username` 值为 "sam"
* `@role` 值为 "monitor"
* `@email` 值为 "sam@school.com"
* `@mobile` 值为 "42883256"
* `@user` 返回整个鉴权信息，数据类型是一个数据行，可以通过 **FOR语句** 遍历，也可以通过点属性法获取属性，如 `@user.role`。

### 使用用户名和角色对接口进行访问控制

除了能在接口逻辑中使用这些用户信息以外，还有可通过用户名`username`和角色`role`对接口进行访问控制，批量控制参见[OneApi访问控制](/oneapi/permit.md)，单个接口访问控制参见[编辑接口](/oneapi/edit.md)。


---
参考链接
* [OneApi全局设置](/oneapi/setup.md)
* [OneApi访问控制](/oneapi/token.md)
* [OneApi访问控制](/oneapi/permit.md)
* [创建和编辑接口](/oneapi/edit.md)