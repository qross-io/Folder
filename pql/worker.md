# PQL执行器 - Worker 

**Worker** 是 PQL 的独立执行器，可以执行 PQL 过程文件和语句。Worker 本身是一个 jar 包，通过接收各种参数来实现相应的执行功能。

```sh
/usr/java/jdk1.8.0_251/bin/java -jar /usr/qross/qross-worker-1.1.0.jar -file /usr/qross/test.sql
```

## Worker 项目及源码

因为 Worker 项目只引入了 4 种常用关系数据库和 Redis 的依赖，如果要连接其他的 JDBC 数据源，需要自己引入依赖然后打包。项目源代码地址为：

[https://github.com/qross-io/Worker](https://github.com/qross-io/Worker)

### 项目文件

项目核心只有一个文件，在项目中分别使用 Java 和 Scala 实现。Java 文件路径为`io.qross.worker.Main`，这个文件只说明功能，本身未被项目使用；Scala 文件路径为`io.qross.worker.Worker`，是项目的入口文件。

### 环境要求

* Intellij Idea 2018 或以上版本（强烈建议）
* JDK 1.8 或以上版本（必须）
* Scala 2.12 或以上版本（可选）
* Gradle 4.9 或以上版本（如果需要可自行修改成 Maven）

如果你不会 Java 或 Scala 开发，或者改变环境非常烦琐，可联系作者进行打包。

## Worker 参数列表

### -file path.sql

执行 PQL 文件, `path`为完整路径, 如`/path/test.sql`, 路径中间如果有空格, 需要加双引号, 如`-file "c:/sql files/test.sql"`。

### -debug on

是否在运行 PQL 时默认打开调试模式，调试模式可以输出更多的日志，调试模式默认是关闭的。接受任意可识别为`true`的值，如`True`、`1`、`yes`、`on`等，不区分大小写。

### -log text 

日志的输出格式，默认`text`，输出文本日志。另一个可选值是`html`，输出 HTML 格式的日志，在网页上显示时比较有用。

### -sql "SELECT * FROM table1"

执行 PQL 语句, 因为有空格, 所以需要用引号把语句引起来, 不建议用这种方式执行过长的 SQL 语句。注意 PQL 语句中的双引号需要转换成`~u0034`。

### -var|-args name1=value1&b=2

PQL 语句的入参, 格式同 URL 地址参数规则, 参数名和参数值之间使用等号`=`分隔, 参数和参数之间使用与号`&`分隔。如果使用`-file`运行文件，也可以直接附在文件名后面，例如`-file /usr/qross/test.sql?id=1&name=Tom`。参数名`-var`和`-args`二选一
    
### -login userid=1&username=Tom&role=monitor&email=tom@school.com

可以将用户信息传入 PQL，在 PQL 过程中可以全局变量调用这些鉴权信息，如变量`@role`可以得到`monitor`。如果你的 PQL 过程需要根据不同的用户执行不同的操作时尤其重要。

### -note 24

[Keeper](/keeper/overview) 调度即时查询的命令，如果使用到了 [Master](/master/overview.md) 的数据管理和即时查询功能请必须保留。

### -job 51

[Keeper](/keeper/overview) 调度 PQL 任务的调度 ID，在使用 Keeper 调度你的任务时使用。

### -task 23476

[Keeper](/keeper/overview) 调度 PQL 任务的命令实例 ID，即 Action ID，在使用 Keeper 调度你的任务时使用。

### -url jdbc:mysql://localhost:3306/qross

设置一个 JDBC 连接串以进行连接测试。需要配合`-driver`等参数使用，测试返回成功或失败的信息。

### -driver com.mysql.jdbc.Driver

设置`-url`参数连接测试需要用到的 JDBC 驱动。

### -username 用户名

设置`-url`参数连接测试所需要的用户名。

### -password 密码

设置`-url`参数连接测试所需要的密码。

## 更简单的方式使用 Worker

因为 Worker 是一个 jar 包，如果你需要在服务器上经常输入命令执行会相当麻烦。可以将 Worker 修改成一个 Shell 命令，如

```sh
pql -file /usr/qross/pql/test.sql
```

操作步骤如下：

1. 创建名为`pql.sh`的文件，Qross系统中已经有这个文件。

    ```sh
    vim pql.sh
    ```

2. 在打开的编辑器中，按`I`键输入下面的命令并按`ESC`然后输入`:wq`保存并退出。

    ```sh
    /usr/java/jdk1.8.0_251/bin/java -jar /usr/qross/qross-worker-2.4.0.jar $*
    ```

    其中 java 目录根据你的安装确定，使用`which java`命令可以查看。Worker 目录也修改成 Worker jar 包的保存位置。

3. 输入`cd /usr/local/bin`切换目录。

4. 在`/usr/local/bin`目录下创建软链接。

    ```sh
    ln -s /usr/qross/pql.sh pql
    ```

5. 完成，输入`pql`命令进行测试。


---
参考链接

* [PQL 概览](/pql/overview.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [数据管理平台 Master](/master/overview.md)