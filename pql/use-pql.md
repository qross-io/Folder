# 使用PQL

## 运行环境

因为 PQL 是一种脚本语言，所以其运行环境非常宽松。PQL 可以在 Java 和 Scala 中像运行 SQL 一样调用（需要引入依赖），也可以通过 Shell 命令接运行 PQL 文件，还可以在 [Keeper 调度工具](/keeper/overview.md)中直接运行。PQL 还有两个扩展应用，[OneApi 统一接口](/oneapi/overview.md)和 [Voyager 模板引擎](/voyager/overview.md)，都基于 Spring Boot 环境运行。另外，[DataHub 开发框架](/datahub/overview.md)作为 PQL 的底层，也可以运行 PQL 过程。

## 在线编辑和运行PQL

如果你不会 Java 或 Scala，可以在本机或服务器上部署 **Qross Master** 项目，在线编辑和运行 PQL。Master 项目部署和运行请参阅 [Master 数据管理平台](/master/overview.md)，也可先[线上体验](http://m.qross.cn)。

## 部署 PQL 开发环境

### 在 Intellij Idea 中开发 PQL

如果你会 Java 或 Scala，作者建议使用 Intellij Idea 作为 PQL 的开发环境，需要 Intellij Idea 2018 或以上版本。你可以在 [Github](https://github.com/qross-io/PQLExample) 上下载一个示例项目。示例项目的环境要求如下：

* JDK 1.8 或以上版本
* Scala 2.10 或以上版本（可选）
* Gradle 4.9 或以上版本（可自行修改为 Maven）

### 引入PQL依赖

PQL 可以在任何 Java 和 Scala 项目里使用，包括 Spring Boot 项目，只要引入一个依赖即可。[OneApi 统一接口](/oneapi/overview.md)和 [Voyager 模板引擎](/voyager/overview.md)也一样引用这个依赖。PQL 项目暂时使用阿里云私有仓库保存，未来会迁移到 Maven 中央仓库。

* **Maven配置如下**

1. settings.xml 中配置凭证

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
2. porn.xml 仓库配置

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

3. porn.xml 依赖配置

```xml
<dependencies>
    <dependency>
        <groupId>io.qross</groupId>
        <artifactId>pql</artifactId>
        <version>0.6.4-40-SNAPSHOT</version>
    </dependency> 
</dependencies>
```

* **Gradle 配置如下**

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
    compile (group: 'io.qross', name: 'pql', version: '1.1.0-RELEASE')
}
```

依赖引入之后，可以测试一下依赖能否正常工作。

```java
io.qross.pql.PQL.run("PRINT 'HELLO WORLD!'");
```

## 配置数据源

PQL 主要用于数据查询和计算，所以在使用之前，需要先配置数据源。PQL 支持多种方式配置数据连接，请参照 [数据连接配置](/pql/properties.md)。


## 编写第一个 PQL 过程

### 编写 PQL 文件
上面的备工作做好之后，下面我们开始编写我们的第一个 PQL 程序。

1. 在项目的`resources`目录下，创建名字的文件夹进行组织，如`pql`。
2. 在文件夹下创建扩展名为`sql`的文件，如`test.sql`（PQL本质上还是 SQL 语句的集合）。
3. 打开刚刚创建的文件，输入以下代码：
   ```sql
   DEBUG ON; -- 启用调试
   OPEN mysql.qross; -- 打开一个在properties文件中设置的数据库
   SELECT * FROM table1; -- 写一个查询某个表的SELECT语句
   ```
4. 在 Java 主类的`main`方法中输入调用方法：
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

**io.qross.pql.PQL** 类还有很多重载和方法用于应对各种场景，请参阅 [PQL 类文档](/pql/class.md)。这种方式特别适合于数据计算脚本的开发，甚至不需要写 Java 和 Scala 代码。脚本开发调试完成之后，直接将 PQL 文件部署到服务器上即可，不需要编译和构建。在服务器上可以通过命令直接运行或者通过 [Keeper 调度工具](/keeper/overview.md)运行。作者所在的数据部门采用这种开发方式，非常方便。


### 直接运行PQL过程字符串

除了将 PQL 保存为文件外，也可以直接运行PQL过程字符串。

Java 代码：
```java
public static void main(String[] args) {
    String pql = "DEBUG ON;";
    pql += "OPEN mysql.qross;";
    pql += "SELECT * FROM table1;";

    io.qross.pql.PQL.run(pql);
}
```

Scala 代码：
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

这种方式就和运行 SQL 语句的方式一样，不过这种方式采用的不多，因为每个 PQL 过程都是一个完整的处理逻辑，不是某个程序的一部分。

## 在 Linux 服务器上运行

可以通过 **Jenkins** 或 **FTP** 方式将脚本上传到服务器，需要安装`JDK 1.8`或以上版本。

PQL 提供了 **Worker** 执行器运行 PQL 脚本文件，本身是一个 jar 包，可以自己下载源代码添加自己需要的依赖后自己打包。

```sh
java -jar qross-worker-0.6.3.jar --file /usr/qross/pql/test.sql
```

在本地也可以通过这种方式运行，需要指定文件的绝对路径。可参阅 [Worker 文档](/pql/worker.md)获取更多信息。

## PQL应用

### 统一接口 OneApi

**OneApi** 是 PQL 的一种应用，可以通过 PQL 编辑 Web 服务的接口，以帮助后端工程师减少大量的重复代码编写工作，快速实现业务逻辑。请参阅 [OneApi 文档](/oneapi/overview.md)获取更多信息。

### 模板引擎 Voyager

**Voyager** 是 PQL 的另一个应用，和 OneApi 一样，也是应用于 Web 开发。其同类是 FreeMarker 或 Thymeleaf，不过更简单直接，不需要记各种标记语法，直接在网页 HTML 代码中写 SQL 语句进行查询并输出数据。Voyager 比同类产品支持更多功能，请参阅 [Voyager 文档](/master/overview.md)获取更多信息。

### 任务调度工具 Keeper

**Keeper** 是一个轻量级但功能强大的任务调度工具，支持运行 PQL 任务，也支持通过 PQL 扩展 Keeper，如事件、依赖等。请参阅 [Keeper 文档](/keeper/overview.md)获取更多信息。

### 数据管理平台 Master

**Master** 是一个综合各种数据开发相关功能的管理平台，可以在上面编写 PQL 并直接运行、部署调度任务、管理服务接口等。请参阅 [Master 文档](/master/overview.md)获取更多信息。

---
参考链接

* [io.qross.pql.PQL 类](/pql/class.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [数据管理平台 Master](/master/overview.md)
* [PQL执行器 Worker](/worker/overview.md)