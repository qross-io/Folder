# io.qross.pql.PQL 类

PQL类是整个PQL语句的入口类，Qross项目的其他应用也与PQL类密不可分。PQL类包含了一些简单的重载和方法用于执行PQL过程。

```scala
import io.qross.pql.PQL

PQL.run("PRINT 'HELLO WORLD!'")
PQL.runFile("/usr/qross/pql/test.sql")
```
上例两行代码是最简单的方法，PQL类还提供了其他方法进行更多设置。注意，使用PQL类需要先引入PQL依赖，详见[使用PQL](/pql/use-pql.md)。

### PQL 构造函数
```scala
class PQL(originPQL: String, dh: DataHub) 
```
构造函数一般不会直接使用，多用静态方法。

### PQL 静态方法
Scala代码为
```scala
def open(PQL: String): PQL
def openEmbedded(PQL: String): PQL
def openFile(path: String): PQL
def openEmbeddedFile(path: String): PQL

def run(PQL: String): Any
def runEmbedded(PQL: String): Any
def runFile(path: String): Any
def runEmbeddedFile(path: String): Any
```
Java代码为
```java
public static PQL open(String PQL)
public static PQL openEmbedded(String PQL)
public static PQL openFile(String path)
public static PQL openEmbeddedFile(String path)

public static Object run(String PQL);
public static Object runEmbedded(String PQL);
public static Object runFile(String path);
public static Object runEmbeddedFile(String path);
```
* `open`方法只打开PQL语句或文件，但不运行，返回PQL类。
* `run`方法打开PQL语句或文件并运行，返回运行PQL的结果。
* `Embedded`是嵌入式PQL，用于模板引擎，是[Voyager](/voyager/overview.md)的基础。

### 向PQL传递参数
Scala代码：
```scala
def place(name: String, value: String): PQL
def place(args: (String, String)*): PQL
def place(map: java.util.Map[String, String]): PQL
def place(args: Map[String, String]): PQL
def place(queryString: String): PQL
def placeDefault(defaultValue: String): PQL
```
Java代码：
```java
public PQL place(String name, String value);
public PQL place(Map<String, String> map);
public PQL place(String queryString);
public PQL placeDefault(String defaultValue);
```

要执行的PQL过程可以接受外部传参，见 [向PQL过程传递参数](/pql/params.md)，就是通过这个方法向PQL过程传递参数。分别可以接受单个值、Map键值对、或者查询字符串（格式 name=Tom&age=18&score=89）。`placeDefault`方法的作业时当变量不存在时才赋值，主要用于 OneApi 中的传参。

### 设置PQL过程的变量并赋值
Scala代码：
```scala
def set(variableName: String, value: Any): PQL
def set(variables: (String, Any)*): PQL
def set(row: DataRow): PQL
def set(queryString: String): PQL
def set(model: java.util.Map[String, Any]): PQL
```
Java代码：
```java
public PQL set(String variableName, Object value);
public PQL set(DataRow row);
public PQL set(String queryString);
public PQL set(Map<String, Object> model);
```

通过这个方法可以为PQL过程预设一些变量，在PQL过程中可以使用。和place方法一样，也接受单个变量、多个键值对组成的多个变量及查询字符串，还接受把整个数据行DataRow的数据传给PQL当变量（数据行DataRow中的每一项都是一个变量）。与参数不同，变量是有自己类型的，参数都是字符串类型。


### 把鉴权信息传给PQL
有时我们应用需要用户验证，以根据用户的角色控制权限。PQL提供了登录功能，但不验证，只是接收登录信息，以方便用户在编写PQL时能得到用户的登录信息。  
Scala代码：
```scala
def signIn(userId: Int, userName: String, role: String, info: (String, Any)*): PQL
def signIn(info: (String, Any)*): PQL
def signIn(info : java.util.Map[String, Any]): PQL
def signIn(userId: Int, userName: String, role: String, info : java.util.Map[String, Any]): PQL
```
Java代码：
```java
public PQL signIn(Map<String, Object> info);
public PQL signIn(int userId, String userName, String role, Map<String, Object> info);
```

除了`userId`, `username`和`role`三项基本信息以下，可以通过Map结构向PQL传递更多的鉴权信息。如果你的Sping Boot项目里应用了Spring Security，可以这样`PQL.open(pql).signIn(UserWebAuthenticationDetails.getCredential())`一次性把鉴权信息传入。鉴权信息在PQL中以全局变量的方式访问，如`@userid`、`@username`、`$role`等，Map结构中的鉴权信息也是通过全局变量的方式访问。

### 执行PQL并返回结果
Scala代码：
```scala
def run(): Any
```
Java代码：
```java
public Object run()
```

### 完整示例
Scala代码：
```scala
PQL.openFile('/usr/qross/pql/test.sql')
    .place("name=Tom&age=18&score=89")
    .set("grade=3&rate=A")
    .signIn(1, "Ted", "teacher", "mobile" -> "138****0220", "email" -> "ted@**school.com")
    .run()
```
Java代码：
```java
PQL.openFile('/usr/qross/pql/test.sql')
    .place("name=Tom&age=18&score=89")
    .set("grade=3&rate=A")
    .signIn(1, "Ted", "teacher", 
        new HashMap<String, Object>(){{
            put("mobile", "138****0220");
            put("email", "ted@**school.com")
        }})
    .run();
```

关于PQL类的返回结果，请参阅[OUTPUT语句](/pql/output.md)。


---
参考链接

* [使用PQL](/pql/use-pql.md)
* [向PQL过程传递参数](/pql/params.md)
* [用户变量](/pql/variable.md)
* [全局变量](/pql/global.md)
* [返回结果 OUTPUT](/pql/output.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [PQL执行器 Worker](/worker/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)