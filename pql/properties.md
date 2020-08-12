# 数据源配置
PQL主要用于数据查询和计算，所以在使用之前，需要先配置数据源。PQL使用最简单的`properties`文件进行配置，数据源和配置信息可以保存在以下几个地方。
1. 项目内`resources`目录下的`conf.properties`文件中。
2. 与jar包相同目录（一般为Qross系统家目录）的`qross.properties`文件中。如果是开发中的项目，以Intellij Idea为例，放在项目的`/out/production`目录下。
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
```
OPEN mysql.qross;
```
系统会自动识别该使用哪种驱动程序，但是要求在项目中必须事先引入了相应的依赖。PQL默认引用了MySQL和Redis依赖。数据源名字前面的数据类型前缀`mysql.`不强制使用，但是在多类型数据源系统中建议使用以进行区分。
有的数据源连接串不能写成一行的形式，如Oracle，这样可以使用完整的格式进行配置。
```
oracle.test.url=jdbc:oracle:thin:@127.0.0.1:1521:test
oracle.test.username=sys
oracle.test.password=diablo
oracle.test.driver=oracle.jdbc.driver.OracleDriver
```
特别的，连接名`jdbc.default`用来设置当前项目默认的连接名，意义和MyBatis中的默认连接一样，建议设置。如果已经在MyBatis中设置，则不需要重复设置。
```
jdbc.default=jdbc:mysql://localhost:3306/qross?user=root&password=diablo&useUnicode=true
```
默认连接在OPEN和SAVE语句中使用，如`OPEN DEFAULT;`或`SAVE TO DEFAULT;`。 
 
另外一个保留名称是`mysql.qross`，如果你的项目用到了Qross系统，那么必须配置这个连接。在OPEN语句中这样使用：`OPEN QROSS;`。  

Redis也可以使用类似的方式进行配置。
```
redis.qross.host=localhost
redis.qross.port=6379
redis.qross.password=
redis.qross.database=0
```
在打开Redis时这样写：
```
OPEN REDIS qross;
```

系统用到的其他配置相应的模块会介绍，完整信息见[全局设置](/doc/pql/setup)。

---
参考链接
* [PQL全局设置](/doc/pql/setup)
* [打开和切换数据源 OPEN](/doc/pql/open)
* [使用PQL](/doc/pql/use-pql)