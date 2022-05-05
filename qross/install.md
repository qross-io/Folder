# 安装 Qross 系统

**Qross** 系统因为组件比较多，安装时需要注意一下顺序，总体来说比较简单。

## 环境要求

**Qross** 系统环境建议如下：

* 任意版本的`Linux`操作系统
* `MySQL 5.7`或更高版本
* `JDK 1.8`或更高版本

**Qross** 系统理论上需要运行在 Linux 环境下，但出于开发调试的目的，Qross 系统也可以运行在 Windows 环境下，但会缺少某些附加功能。 MySQL 版本要求不强制，理论上 MariaDB 也可以，作者在开发过程中尽量规避了数据库版本之间的差异。JDK 是必须的。

## 文件清单

需要有专门的目录保存 Qross 系统的所有文件，如`/usr/qross`。将压缩包解压之后把所有相关文件放在这个专用目录下。Qross 系统的文件清单如下：

* `qross-master-x.x.x.jar` Master 管理平台，是一个 Spring Boot 项目，可以单独启动。
* `qross-keeper-x.x.x.jar` Keeper 任务调度工具。
* `qross-worker-x.x.x.jar` PQL 执行器。如果生产中用到了除了 4 种常用关系数据库之外的数据库，需要[下载源码](https://github.com/qross-io/Worker)然后添加相关依赖重新打包或联系作者打包。
* `qross.properties` 配置文件，主要保存数据连接和其他配置项，**非常重要**!
* `data` 目录，保存需要初始化的数据文件，系统初始化后可删除。
* `qross-keeper-start.sh` Shell 脚本，在`crontab`配置时使用。

其中的`x.x.x`表示版本号，最新版本为`1.8.0`。

## 配置数据源

在`qross.properties`文件中添加生产过程中需要的数据源，其中两个配置项必须有：

* `mysql.qross` 这个是 Qross 系统用到的 MySQL 数据库连接配置。
* `jdbc.default` 这个是系统要连接默认 JDBC 数据源，可以是任意数据库。

建议只在`qross.properties`文件中只配置这两个数据源，其他数据源配置可根据需要在系统设置的“数据库连接”中添加，建议名称前带数据库类型前缀，如`hive.default`、`oracle.prod`等。不能写成一行的数据库连接串，可以这样写：

```properties
oracle.test.url=jdbc:oracle:thin:@127.0.0.1:1521/test
oracle.test.username=sys
oracle.test.password=diablo
oracle.test.driver=oracle.jdbc.driver.OracleDriver
```

## 初始化 Qross 数据库

保证数据库能正常连接，使用`java -jar qross-master-x.x.x.jar`命令启动 **Master**。在浏览器中访问相应的网址如`http://localhost:8080`，在打开的页面上输入管理员用户名密码然后点击按钮在数据库中创建 Qross 系统。需要等待一会儿，创建过程中不要关闭或刷新网页。

Master 是 Spring Boot 项目，可以为其分配相应的域名和端口，默认端口为`8080`，如可通过`--server.port=80`参数设置为其他端口。

## 进行必要的设置

这些设置可以在欢迎页会“系统”模块中可以找到。

* **公司名称** 这是系统的名称标识。
* **Qross 系统目录** 即存放 Qross 各个文件的目录，这里会保存 Qross 系统相关的所有信息。在创建系统时会自动设置，可以检查一下是否正确。
* **Java Bin Home** 如果会调度 Java 程序，需要设置。
* **Python2/Python3 Home** 如果会用到 Python，需要设置。
* **邮箱发件人设置** 邮件在系统中有很多用途，如通知、预警等，建议设置。

另外，用户或权限等其他配置可根据需求进行添加或设置。

## 配置`crontab`并启动 Keeper

如果需要使用 Keeper 进行任务调度，这个步骤需要配置。

首先先编辑一下`qross-keeper-start.sh`文件，其中的一行：

```sh
/srv/jdk1.8/bin/java -cp /usr/qross/qross-keeper-x.x.x.jar io.qross.keeper.Protector
```

首先需要把`/usr/qross`部分修改为 Qross 系统的保存目录。然后要把`/srv/jdk1.8`修改为 JDK 的安装目录。最后需要修改版本号到对应版本。

使用`crontab -e`命令，在打开界面中添加一行，注意路径：

```sh
*/1 * * * * /usr/qross/qross-keeper-start.sh
```

配置完成后 Keeper 会在下一分钟后自动启动。Windows 环境因为没有或不配置 crontab，可忽略此步骤，使用`java -cp qross-keeper-x.x.x.jar io.qross.keeper.Protector`启动 Keeper。

上面的`x.x.x`部分记得替换成相应的版本号。

自此，Qross 系统安装并启动完成。

## 多机集群配置

Keeper 支持以集群方式运行，可以在局域网的不同的机器上运行 Keeper。在第一台机器上运行起来后，其他机器的配置就相对简单一些。

* 设置与第一台机器相同的 Qross 系统目录，将相关文件如`qross-keeper-x.x.x.jar`存放在这里，包括已配置好的`qross.properties`文件。不同机器的 Qross 系统目录必须保证相同。
* 配置`crontab`，与第一台机器一样。
* crontab 会自动启动 Keeper。

Keeper 以 **对等集群** 模式运行，不需要再进行其他额外的设置，新启动的机器会自动加入集群。唯一需要确认的是不同的`qross.properties`中配置的`mysql.qross`连接指向同一个数据库即可。Keeper 默认使用`7700`端口，如果要修改端口，可以在“系统”界面中设置或在启动时指定。关于集群模式的更多信息请参阅 [Keeper 集群模式](/keeper/cluster.md)

```sh
java -cp qross-keeper-x.x.x.jar io.qross.keeper.Protector 7701
```

## 其他可用的资源

* [使用 PQL 语言进行数据开发](/pql/use-pql)
* [使用 OneApi 快速开发接口](/oneapi/quick)
* [Keeper 相关文档](/keeper/overview)
* [Keeper 多机集群模式](/keeper/cluster.md)
* [Worker 文档](/pql/worker)
* [Master 相关文档](/master/overview)