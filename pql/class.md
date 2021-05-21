# io.qross.pql.PQL 类

PQL 类是整个 PQL 语句的入口类，Qross 项目的其他应用也与 PQL 类密不可分。PQL 类包含了一些简单的重载和方法用于执行 PQL 过程。

```scala
import io.qross.pql.PQL

PQL.run("PRINT 'HELLO WORLD!'")
PQL.runFile("/usr/qross/pql/test.sql")
```

上例两行代码是最简单的方法，PQL 类还提供了其他方法进行更多设置。注意，使用 PQL 类需要先引入 PQL 依赖，详见[使用 PQL](/pql/use-pql.md)。

### PQL 构造函数

```scala
class PQL(originPQL: String)
class PQL(originPQL: String, dh: DataHub) 
class PQL(originPQL: String, embedded: Boolean)
class PQL(originPQL: String, embedded: Boolean, dh: DataHub)
```

```java
class PQL(String originPQL)
class PQL(String originPQL, DataHub dh) 
class PQL(String originPQL, boolean embedded)
class PQL(String originPQL, boolean embedded, DataHub dh)
```

构造函数一般不会直接使用，多用静态方法。

### PQL 静态方法

Scala 代码：

```scala
def open(PQL: String): PQL
def openEmbedded(PQL: String): PQL
def openFile(path: String): PQL
def openEmbeddedFile(path: String): PQL

def run(PQL: String): Any
def runEmbedded(PQL: String): Any
def runFile(path: String): Any
def runEmbeddedFile(path: String): Any

def check(PQL: String): String
def checkFile(path: String): String

def recognizeParameters(SQL: String): java.util.HashSet[String]
def recognizeParametersInFile(path: String): java.util.HashSet[String]
```

Java 代码：

```java
public static PQL open(String PQL)
public static PQL openEmbedded(String PQL)
public static PQL openFile(String path)
public static PQL openEmbeddedFile(String path)

public static Object run(String PQL);
public static Object runEmbedded(String PQL);
public static Object runFile(String path);
public static Object runEmbeddedFile(String path);

public static String check(String PQL);
public static String checkFile(String path);

public static HashSet<String> recognizeParameters(String SQL);
public static HashSet<String> recognizeParametersInFile(String path);
```

* `open`方法只打开 PQL 语句或文件，但不运行，返回 PQL 类。
* `run`方法打开 PQL 语句或文件并运行，返回运行 PQL 的结果。
* `embedded`是[嵌入式 PQL](/pql/embedded.md)，可用于模板引擎，是 [Voyager](/voyager/overview.md) 的基础。
* `check`方法用于检查 PQL 过程是否有语法错误，正确时返回空字符串，出错时返回第一个错误的文本信息。
* `recognizeParameters`方法用于识别 PQL 过程中的参数。

### 向 PQL 传递参数

Scala 代码：

```scala
def place(name: String, value: String): PQL
def place(args: (String, String)*): PQL
def place(map: java.util.Map[String, String]): PQL
def place(args: Map[String, String]): PQL
def place(queryString: String): PQL
def placeDefault(defaultValue: String): PQL
```
Java 代码：
```java
public PQL place(String name, String value);
public PQL place(Map<String, String> map);
public PQL place(String queryOrJsonString);
public PQL placeDefault(String defaultValue);
```

要执行的 PQL 过程可以接受外部传参，见 [向 PQL 过程传递参数](/pql/params.md)，就是通过这个方法向 PQL 过程传递参数。分别可以接受单个值、Map 键值对、或者查询字符串（格式 name=Tom&age=18&score=89）、Json 对象字符串等形式。`placeDefault`方法的作业时当变量不存在时才赋值，主要用于 OneApi 中的传参。

### 设置 PQL 过程的变量并赋值

Scala 代码：

```scala
def set(variableName: String, value: Any): PQL
def set(variables: (String, Any)*): PQL
def set(row: DataRow): PQL
def set(queryString: String): PQL
def set(model: java.util.Map[String, Any]): PQL
```

Java 代码：

```java
public PQL set(String variableName, Object value);
public PQL set(DataRow row);
public PQL set(String queryString);
public PQL set(Map<String, Object> model);
```

通过这个方法可以为 PQL 过程预设一些变量，在 PQL 过程中可以使用。和`place`方法一样，也接受单个变量、多个键值对组成的多个变量及查询字符串，还接受把整个数据行`DataRow`的数据传给 PQL 当变量（数据行`DataRow`中的每一项都是一个变量）。与参数不同，变量是有自己类型的，参数都是字符串类型。

`place`和`set`方法区别为：`place`方法替换参数占位符`#{param}`并且附加同名变量，例如`place`的参数值为`name=Tom`，那么会替换`#{name}`为`Tom`并且赋值一个变量`$name`为`Tom`；而`set`方法只赋值变量`$name`为`Tom`，没有其他操作。即`set`是`place`的子操作。

### 把鉴权信息传给 PQL

有时我们应用需要用户验证，以根据用户的角色控制权限。PQL 提供了登录功能，但不验证，只是接收登录信息，以方便用户在编写 PQL 时能得到用户的登录信息。  

Scala 代码：

```scala
def signIn(userId: Int, userName: String, role: String, info: (String, Any)*): PQL
def signIn(info: (String, Any)*): PQL
def signIn(info : java.util.Map[String, Any]): PQL
def signIn(userId: Int, userName: String, role: String, info : java.util.Map[String, Any]): PQL
```

Java 代码：

```java
public PQL signIn(Map<String, Object> info);
public PQL signIn(int userId, String userName, String role, Map<String, Object> info);
```

除了`userId`, `username`和`role`三项基本信息以下，可以通过Map结构向PQL传递更多的鉴权信息。如果你的Sping Boot项目里应用了Spring Security，可以这样`PQL.open(pql).signIn(UserWebAuthenticationDetails.getCredential())`一次性把鉴权信息传入。鉴权信息在PQL中以全局变量的方式访问，如`@userid`、`@username`、`$role`等，Map 结构中的鉴权信息也是通过全局变量的方式访问。

### 执行 PQL 并返回结果

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

关于 PQL 类的返回结果，请参阅 [OUTPUT 语句](/pql/output.md)。


---
参考链接

* [使用 PQL](/pql/use-pql.md)
* [向 PQL 过程传递参数](/pql/params.md)
* [用户变量](/pql/variable.md)
* [全局变量](/pql/global-variable.md)
* [返回结果 OUTPUT](/pql/output.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [PQL执行器 Worker](/worker/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)