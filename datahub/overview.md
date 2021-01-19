# DataHub 概览

**DataHub** 是一个跨数据源的数据处理框架，提供了丰富的数据输入输出功能，是[PQL语言](/pql/overview.md)的底层支持。DataHub 极大的简化了各种数据处理操作，最小化数据处理过程中的附加代码编写，基于 DataHub 开发的[Keeper调度工具](/keeper/overview.md)只有3000行代码。DataHub 同时整合了大量的公共基础类，方便在各业务场景中使用。

DataHub 框架主要使用 **Scala** 语言编写，Scala 项目可以无障碍使用，为了能与 Java 项目通用，一些关键类作者也会提供 **Java** 编程接口。

DataHub 主要功能包列表如下：

* io.qross.core 核心库，主要包含 DataHub 类、DataTable 和 DataRow 数据结构等。
* io.qross.ext 扩展功能包，主要是对基本类型的扩展。
* io.qross.fs 文件操作，文件读写、Excel操作等都在这里。
* io.qross.jdbc 数据库访问，提供多数据源访问支持。
* io.qross.net 网络相关，提供Redis访问、Json处理、Email等功能。
* io.qross.script 多语言脚本处理，支持执行Javascript和Shell。
* io.qross.security 安全和加密。
* io.qross.setting 读取和设置所有的环境变量。
* io.qross.thread 多线程辅助。
* io.qross.time 日期时间相关操作，如Cron表达式等。


DataHub 跟 [PQL 语言](/pql/overview.md)同属于一个依赖包，引用依赖的方法见[使用 PQL](/pql/use-pql.md)。即只要引用同一个依赖，可同时使用 DataHub 和 PQL。

DataHub 免费开源，与 PQL 同属一个开源项目，源码地址为：<https://github.com/qross-io/PQL>