# PQL 全局设置
相对于其他的开发框架来说，PQL的全局设置比较少。除了数据源连接设置以外，PQL还有一些其他的一些简单的设置。这些设置和数据源连接设置一样，都可以放在项目资源目录的`conf.properties`文件中，或项目运行环境jar包同级目录（在运行环境下，一般为Qross系统家目录）的`qross.properties`文件中。如果使用了Qross系统，“全局设置”可以系统管理里在[Master平台](/master/overview.md)中的“系统”中进行设置。配置的优先级顺序：数据库配置优先于`conf.properties`文件，`conf.properties`文件优先于`qross.properties`文件。

另外，全局设置的每一项都对应一个[全局变量](/pql/global.md)。下面对这些设置项分别介绍一下。

### Qross系统家目录
```s
qross.home=/usr/qross/
```
一般在Qross家目录下保存Qross系统所有的程序和相关文件，如`qross-keeper-0.6.3.jar`、`qross.properties`等。PQL临时目录和Excel模板、Email模板等都在Qross家目录下。请参阅[Qross系统家目录](/qross/home.md)获取更多信息。对应的全局变量为`@QROSS_HOME`。

### 默认字符编码
```s
charset=utf-8
```
默认值是`utf-8`，一般不需要设置。这个值影响运行环境相关的所有地方，如文件读写、jar包运行环境等。对应的全局变量为`@CHARSET`。

### 时区
```s
timezone=GMT+8
```
默认值是系统时区，除非获得的时间不正常，一般不需要设置。对应的全局变量为`@TIMEZONE`。

### 全局调试
```s
pql.debug=on
```
默认关闭`off`，开启时能够输出更多的调试日志。这个设置可以由[DEBUG语句](/pql/debug.md)临时开启或关闭。对应的全局变量为`@DEBUG`。这个设置同时影响[DataHub开发框架](/datahub/overview.md)。

### 系统邮件发送账户
这个设置为[SEND语句](/pql/send.md)提供默认的发件人，同时影响Qross系统的其他工具软件，如[Keeper调度工具](/keeper/overview.md)也是使用的这个设置。
```s
email.smtp.host=smtp.exmail.qq.com
email.smtp.port=465
email.sender.personal=PQL
email.sender.account=test@qross.io
email.sender.password=*******
```
这些信息的设置由你的邮件服务商提供，如果你需要使用`SEND语句`或发送邮件功能，建议设置。
* `email.smtp.host` SMTP服务器域名或IP地址
* `email.smtp.port` SMTP服务器端口，一般为`25`，启用SSL时为`465`
* `email.sender.personal` 发件人署名
* `email.sender.account` 发件人账号
* `email.sender.password` 发件人密码
  
### 其他应用的设置
DataHub开发框架使用与PQL一样的系统设置。如果你的服务器上部署了Qross系统，可以在[Master平台](/master/overview.md)的“系统”中看到所有设置。Qross系统设置优先于`properties`文件设置，所以如果你有Qross系统，这些设置完全都可以通过Master管理平台进行。

其他应用如[统一接口OneApi](/oneapi/overview.md)或[调度管理工具Keeper](/keeper/overview.md)还有自己的专有设置项，分别在自己的应用文档中会介绍。

---
* [发送邮件 SEND](/pql/send.md)
* [将数据保存为Excel文件](/pql/excel.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [数据开发框架 DataHub](/datahub/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)