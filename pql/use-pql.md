# 使用PQL

## 运行环境
因为PQL是一种脚本语言，所以其运行环境非常宽松。PQL可以在Java和Scala中像运行SQL一样调用（需要引入依赖），也可以通过Shell命令接运行PQL文件，还可以在[Keeper调度工具](/keeper/overview.md)中直接运行。PQL还有两个扩展应用，[OneApi统一接口](/oneapi/overview.md)和[Voyager模板引擎](/voyager/overview.md)，都基于Spring Boot环境运行。另外，DataHub开发框架作为PQL的底层，也可以运行PQL过程。

## 在线编辑和运行PQL
如果你不会Java或Scala，可以在本机或服务器上部署 **Qross Master** 项目，在线编辑和运行PQL。Master项目部署和运行请参阅[Master数据管理平台](/master/overview.md)，也可先[线上体验](http://www.qross.cn:8080)。

## 部署PQL开发环境
### 在Intellij Idea中开发PQL
如果你会Java或Scala，作者建议使用Intellij Idea作为PQL的开发环境，需要 Intellij Idea 2018 或以上版本。   
你可以在[网站](http://www.qross.cn/pql)上下载一个示例项目。示例项目的环境要求如下：

* JDK 1.8或以上版本
* Scala 2.10或以上版本（可选）
* Gradle 4.9或以上版本（可自行修改为Maven）

### 引入PQL依赖
PQL可以在任何Java和Scala项目里使用，包括Spring Boot项目，只要引入一个依赖即可。[OneApi统一接口](/oneapi/overview.md)和[Voyager模板引擎](/voyager/overview.md)也一样引用这个依赖。PQL项目暂时使用阿里云私有仓库保存，未来会迁移到Maven中央仓库。

* **Maven配置如下**
1. settings.xml中配置凭证
```xml
<servers>
  <server>
    <id>rdc-releases</id>
    <username>PtbpNI</username>
    <password>kwCz3C0wHx</password>
  </server>
  <server>
    <id>rdc-snapshots</id>
    <username>PtbpNI</username>
    <password>kwCz3C0wHx</password>
  </server>
</servers>
```
2. porn.xml仓库配置
```xml
<distributionManagement>
  <repository>
    <id>rdc-releases</id>
    <url>https://packages.aliyun.com/maven/repository/2011186-release-Aa5YmC/</url>
  </repository>
  <snapshotRepository>
    <id>rdc-snapshots</id>
    <url>https://packages.aliyun.com/maven/repository/2011186-snapshot-FSoDsK/</url>
  </snapshotRepository>
</distributionManagement>
```
3. porn.xml依赖配置
```xml
<dependencies>
    <dependency>
        <groupId>io.qross</groupId>
        <artifactId>pql</artifactId>
        <version>0.6.4-40-SNAPSHOT</version>
    </dependency> 
</dependencies>
```

* **Gradle配置如下**
```groovy
repositories {
    maven{
		credentials {
			username 'PtbpNI'
			password 'kwCz3C0wHx'
		}
		url "https://packages.aliyun.com/maven/repository/2011186-snapshot-FSoDsK/"
		//url "https://packages.aliyun.com/maven/repository/2011186-release-Aa5YmC/"
    }
}

dependencies {
    compile (group: 'io.qross', name: 'pql', version: '0.6.4-40-SNAPSHOT')
}
```

依赖引入之后，可以测试一下依赖能否正常工作。
```java
io.qross.pql.PQL.run("PRINT 'HELLO WORLD!'");
```

## 配置数据源
PQL主要用于数据查询和计算，所以在使用之前，需要先配置数据源。PQL使用最简单的`properties`文件进行配置，数据源和配置信息可以保存在以下几个地方。

1. 项目内`resources`目录下的`conf.properties`文件中。
2. 与jar包相同目录的`qross.properties`文件中。如果是开发中的项目，以Intellij Idea为例，放在项目的`/out/production`目录下。
3. 如果是已存在的`properties`文件，可以通过 `io.qross.setting.Properties.loadLocalFile('file.properties');` 手动加载。
4. 如果项目应用了MyBatis，PQL会自动读取MyBatis中的数据源信息。
5. 在Qross系统中，可以在设置中管理数据源信息。
6. PQL暂时不支持从注册中心如Eureka读取数据源信息。
7. Yml方式的配置会未来的某个版本支持，现在不是作者的开发重点。

作者建议使用第一种和第二种进行数据源和其他配置，系统的其他配置也保存在`properties`文件中。 
典型的数据源配置如下：
```
mysql.qross=jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false
```
然后可以在PQL编写时这样使用：
```sql
OPEN mysql.qross;
```
系统会自动识别该使用哪种驱动程序，但是要求在项目中必须事先引入了相应的依赖。PQL默认引用了MySQL和Redis依赖。数据源名字前面的数据类型前缀`mysql.`不强制使用，但是在多类型数据源系统中建议使用以进行区分。
有的数据源连接串不能写成一行的形式，如Oracle，这样可以使用完整的格式进行配置。
```properties
oracle.test.url=jdbc:oracle:thin:@127.0.0.1:1521:test
oracle.test.username=sys
oracle.test.password=diablo
oracle.test.driver=oracle.jdbc.driver.OracleDriver
```
特别的，连接名`jdbc.default`用来设置当前项目默认的连接名，意义和MyBatis中的默认连接一样，建议设置。如果已经在MyBatis中设置，则不需要重复设置。
```sh
jdbc.default=jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true
```
默认连接在OPEN和SAVE语句中使用，如`OPEN DEFAULT;`或`SAVE TO DEFAULT;`。 
 
另外一个保留名称是`mysql.qross`，如果你的项目用到了Qross系统，那么必须配置这个连接。在OPEN语句中这样使用：`OPEN QROSS;`。  

Redis也可以使用类似的方式进行配置。
```properties
redis.qross.host=localhost
redis.qross.port=6379
redis.qross.password=
redis.qross.database=0
```
在打开Redis时这样写：
```sql
OPEN REDIS qross;
```

## 编写第一个PQL过程

### 编写PQL文件
上面的备工作做好之后，下面我们开始编写我们的第一个PQL程序。

1. 在项目的`resources`目录下，创建名字的文件夹进行组织，如`pql`。
2. 在文件夹下创建扩展名为`sql`的文件，如`test.sql`（PQL本质上还是SQL语句的集合）。
3. 打开刚刚创建的文件，输入以下代码：
   ```sql
   DEBUG ON; -- 启用调试
   OPEN mysql.qross; -- 打开一个在properties文件中设置的数据库
   SELECT * FROM table1; -- 写一个查询某个表的SELECT语句
   ```
4. 在Java主类的`main`方法中输入调用方法：
   ```java
   public static void main(String[] args) {
      io.qross.pql.PQL.runFile("/pql/test.sql");
   }   
   ```
   Scala代码：
   ```scala
   def main(Array[String] args): Unit = {
      io.qross.pql.PQL.runFile("/pql/test.sql")
   }
   ```
5. 运行主类查看结果。 

io.qross.pql.PQL类还有很多重载和方法用于应对各种场景，请参阅[PQL类文档](/pql/class.md)。这种方式特别适合于数据计算脚本的开发，甚至不需要写Java和Scala代码。脚本开发调试完成之后，直接将PQL文件部署到服务器上即可，不需要编译和构建。在服务器上可以通过命令直接运行或者通过[Keeper调度工具](/keeper/overview.md)运行。作者之前的数据部门采用这种开发方式，非常方便。


### 直接运行PQL过程字符串
除了将PQL保存为文件外，也可以直接运行PQL过程字符串。
Scala代码：
```java
public static void main(String[] args) {
    String pql = "DEBUG ON;";
    pql += "OPEN mysql.qross;";
    pql += "SELECT * FROM table1;";

    io.qross.pql.PQL.run(pql);
}
```
Scala代码：
```scala
def main(Array[String] args): Unit = {
    val pql = 
      """
        DEBUG ON;
        OPEN mysql.qross;
        SELECT * WFROM table1;
      """
    io.qross.pql.PQL.run(pql);
}
```

这种方式就和运行SQL语句的方式一样，不过这种方式采用的不多，因为每个PQL过程都是一个完整的处理逻辑，不是某个程序的一部分。

## 在Linux服务器上运行
可以通过 **Jenkins** 或 **FTP** 方式将脚本上传到服务器，然后有几种方式运行你的PQL脚本。服务器上需要安装`JDK 1.8`或以上版本。

PQL提供了 **Worker** 执行器运行PQL脚本文件，本身是一个jar包，可以从[官网](http://www.qross.cn/worker)上下载，也可以自己下载源代码添加自己需要的依赖后自己打包。
```sh
java -jar qross-worker-0.6.3.jar --file /usr/qross/pql/test.sql
```
在本地也可以通过这种方式运行，需要指定文件的绝对路径。可参阅[Worker文档](/worker/overview.md)获取更多信息。

## PQL应用

### 统一接口 OneApi
**OneApi** 是PQL的一种应用，可以通过PQL编辑Web服务的接口，以帮助后端工程师减少大量的重复代码编写工作，快速实现业务逻辑。请参阅[OneApi文档](/oneapi/overview.md)获取更多信息。

### 模板引擎 Voyager
**Voyager** 是PQL的另一个应用，和OneApi一样，也是应用于Web开发。其同类是FreeMarker或Thymeleaf，不过更简单直接，不需要记各种标记语法，直接在网页HTML代码中写SQL语句进行查询并输出数据。请参阅[Voyager文档](/master/overview.md)获取更多信息。

### 任务调度工具 Keeper
**Keeper** 是一个轻量级但功能强大的任务调度工具，支持运行PQL任务，也支持通过PQL扩展Keeper，如事件、依赖等。请参阅[Keeper文档](/keeper/overview.md)获取更多信息。

### 数据管理平台 Master
**Master** 是一个综合各种数据开发相关功能的管理平台，可以在上面编写PQL并直接运行、部署调度任务、管理服务接口等。请参阅[Master文档](/master/overview.md)获取更多信息。

---
参考链接

* [io.qross.pql.PQL 类](/pql/class.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [数据管理平台 Master](/master/overview.md)
* [PQL执行器 Worker](/worker/overview.md)