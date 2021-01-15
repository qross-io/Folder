# 数据源配置

PQL主要用于数据查询和计算，所以在使用之前，需要先配置数据源。PQL 至少有一个基础配置和其他扩展配置。PQL 的数据源配置可以保存在配置文件中、Nacos 配置中心或从 URL 接口中获取。如果使用了 Qross 系统，管理员还可以在“系统设置”中设置哪里有数据连接或直接配置数据连接。系统支持`3`种格式的配置，分别是 Properties、Yaml 和 Json。

数据连接配置的应用范围是整个 Qross 系统所有应用。

## PQL 基础配置文件

PQL 基础使用最简单的`properties`文件，可以保存在以下两个位置。

1. 项目内`resources`目录下的`conf.properties`文件中。
2. 与 jar 包相同目录（一般为 Qross 系统家目录）的`qross.properties`文件中。如果是开发中的项目，以Intellij Idea 为例，放在项目的`/out/production`目录下。

基础配置文件必须有，整个 Qross 系统都从这里开始。基础配置文件中可以保存数据连接，或者告诉 PQL 在哪里可以找到数据连接。如果在`qross.properties`中配置数据源，同级目录下的其他程序均可使用。`conf.properties`只能当前项目内部使用。`conf.properties`的优先级高于`qross.properties`。

在基础配置文件中，可以设置数据连接文件的位置，就是告诉系统去哪里可以找到数据连接，即可以与其他应用共享配置，分别有以下`9`个设置可用。

```properties
file.properties=/abc/conf.properties;/efg/conf.properties
file.yaml=/abc/application.yml
file.json=
nacos.properties=localhost:8848:io.qross.conf:connections;localhost:8848:io.qross.conf:others;
nacos.yaml=
nacos.json=
url.properties=http://domain.com/cofig?id=1;http://domain.com/cofig?id=2
url.yaml=
url.json=
```

* `file`表示从文件中获取数据连接，`nacos`表示从 Nacos 配置中心获取数据连接，`url`表示从 URL 地址获取数据连接。
* 同一个配置项如果有多项，比如多个文件或地址，使用分号`;`隔开。
* 从文件中获取数据连接时，指定的文件路径可以是绝对路径，也可以是相对项目的 resources 资源目录路径。
* Nacos 配置中心的路径格式为`host:port:group:data-id`。
* 如果使用其他类型的配置中心，可设置 URL 地址，系统根据 URL 地址去获取数据连接配置。

## 配置数据源

下面以 Properties 为例说明一下数据连接的设置，典型的数据源配置如下：

```properties
mysql.qross=jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false
```

然后可以在PQL编写时这样使用：

```sql
OPEN mysql.qross;
```

一般情况下系统会自动识别该使用哪种驱动程序，但是要求在项目中必须事先引入了相应的依赖。不能识别时请手工设置`driver`属性，如：

```properties
jdbc.default.driver=com.facebook.presto.jdbc.PrestoDriver
```

PQL 默认引用了 MySQL 依赖。数据源名字前面的数据类型前缀`mysql.`不强制使用，但是在多类型数据源系统中建议使用以进行区分。
有的数据源连接串不能写成一行的形式，如 Oracle，这样可以使用完整的格式进行配置。

```properties
oracle.test.url=jdbc:oracle:thin:@127.0.0.1:1521/test
oracle.test.username=sys
oracle.test.password=root
oracle.test.driver=oracle.jdbc.driver.OracleDriver
```

特别的，连接名`jdbc.default`用来设置当前项目默认的连接名，意义和 MyBatis 中的默认连接一样，强烈建议设置。如果已经在 MyBatis 中设置`<environments default="qross">`，则不需要重复设置。

```sh
jdbc.default=jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true
```

默认连接在 OPEN 和 SAVE 语句中使用，如`OPEN DEFAULT;`或`SAVE TO DEFAULT;`。 
 
另外一个保留名称是`mysql.qross`，如果你的项目用到了 Qross 系统，那么必须配置这个连接。在 OPEN 语句中这样使用：`OPEN QROSS;`。  

Redis也可以使用类似的方式进行配置。**还需要在项目中引入依赖`jedis 3.0`或以上版本**

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

## Yaml 格式

有的工程师喜欢使用 Yaml 格式进行配置，系统支持 Yaml 格式配置文件。

```
mysql:
    qross: jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false
    db1: jdbc:mysql://localhost:3307/db1?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false
    db2: jdbc:mysql://localhost:3308/db2?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false
```

以上会产生三个连接，连接名分别为`mysql.qross`、`mysql.db1`、`mysql.db2`。

## Json 格式

使用 Json 格式进行数据连接配置的情况较少，有时会在配置中心使用。

```json
{
    "mysql": {
        "qross": "jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false"
        "test": "jdbc:mysql://localhost:3307/test?user=root&password=diablo&useUnicode=true&characterEncoding=utf-8&useSSL=false"
    },

    "oracle": {
        "test": {
            "url": "url=jdbc:oracle:thin:@127.0.0.1:1521/test",
            "username": "sys",
            "password": "root",
            "driver": "oracle.jdbc.driver.OracleDriver"
        }
}
```

上例可产生3个数据连接项，分别为`mysql.qross`、`mysql.test`、`oracle.test`。

## 使用 MyBatis 的连接配置

如果项目应用了 MyBatis，系统会自动读取`/mybatis-config.xml`中的数据源配置信息。如上面提到过，如果设置了 MyBatis 的默认数据源`<environments default="qross">`，则会自动赋给`jdbc.default`。

## 在 Qross 系统库中保存连接配置

如果使了 Qross 系统，则可以在“设置”中管理“数据连接文件”和“数据源连接信息”。系统用户在使用 Qross 系统时，也可以自己添加个人个人数据源连接，这些个人连接会保存在 Qross 数据库中。

## 小结

Qross 提供了多种获取数据连接的方式，可根据需要将数据连接保存在需要的地方。另外，也可以 PQL 运行时加载配置文件，详见 [LOAD 语句](/pql/load.md)。还可以在 PQL 运行时直接通过连接串和驱动打开数据源，详见 [OPEN 语句](/pql/open.md)。系统用到的其他配置相应的模块会介绍，完整信息见[全局设置](/pql/setup.md)。

---
参考链接

* [PQL 全局设置](/pql/setup.md)
* [手动加载配置文件 LOAD](/pql/load.md)
* [打开和切换数据源 OPEN](/pql/open.md)
* [使用 PQL](/pql/use-pql.md)
* [Redis 操作](/pql/redis.md)