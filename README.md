# Qross 系统文档

**Qross** (Query Cross DataSources) 是提速和简化数据开发、后端开发的一整套解决方案，核心组件包含[数据处理语言 PQL](/pql/overview.md)、[任务调度工具 Keeper](/pql/overview.md)、[数据管理平台 Master](/master/overview.md)、[统一接口应用 OneApi](/oneapi/overivew.md)、[数据开发框架 DataHub](/datahub/overview.md)和[前端组件库 root.js](/root.js/overview.md)。Qross 系统使用`Scala`和`Java`语言编写，免费且开源。

## # [过程化查询语言 PQL](/pql/overview.md)

**PQL** (Procedural Query Language) 是一种适合于数据处理中间件语言，语法类似于 Oracle 的`PL/SQL`。PQL 简单易用但功能强大，所有功能基本都可以用一条语句搞定。支持多数据源的数据输入和输出，并支持多个与数据处理相关的周边功能，如 Excel 读写、邮件发送、接口访问等。PQL 适用于数据统计计算、后端接口服务、模板引擎等应用场景。

* 可支持[任何 JDBC 数据源](/pql/properties)，并可在不同数据之间任意[流转数据](/pql/dataflow.md)。
* 提供[缓存数据库](/pql/cache.md)和[中间缓冲区](/pql/get.md)，用于缓冲或临时保存处理过程中的中间数据。
* 支持常见类型的[用户变量](/pql/variable.md)和[全局变量](/pql/global-variable.md)。
* 支持 [Email 发送](/pql/send.md)，可设置正文和签名模板，可添加附件。
* 支持[接口访问](/pql/request.md)，交互数据更简单。
* 支持使用 SQL 对文件查询和输出，如 [Excel](/pql/excel.md)、[Json](/pql/json-file.md)、[CSV](/pql/csv.md)，[TXT](/pql/txt.md)等。
* 支持 [IF](/pql/if.md) 和 [CASE](/pql/case.md) 条件判断。
* 支持 [FOR](/pql/for.md) 和 [WHILE](/pql/while.md) 循环控制。
* 强大而优雅的 [Sharp 表达式](/pql/sharp.md)，多步链式数值加工过程，包含丰富的数据处理方法。
* 支持[自定义函数](/pql/function.md)。
* 丰富的[嵌入功能](/pql/place.md)与原生 SQL 语句完美结合。

PQL 文档已是最新。PQL 的源码地址为：<https://github.com/qross-io/PQL>

## # [任务调度工具 Keeper](/keeper/overview.md)

**Keeper** 是一个轻量级的任务调度工具，是 Qross 系统的第一个项目，已运行和进化多年，相关功能已非常完善。Keeper 的主要功能有：

* 支持时间计划调度、无限循环调度、仅依赖触发的调度和仅手工执行的调度。
* 支持三种前置依赖：任务依赖、SQL 依赖和 PQL 依赖；并支持任务结果校验。
* 支持 DAG，每个 DAG 节点命令支持运行`Shell`、`Python`和`PQL`。
* 支持不同任务状态下触发事件，可实现多种预警功能。
* 支持超时或失败重试和延时自动重启。
* 支持立即运行任务、中断任务、重启任务。
* 支持已扩展的`Cron表达式`设置和自动启停调度。
* 多角色设置，精细的权限控制。
* 丰富的统计图表，随时掌握任务运行状态。
* 超强的自定义功能：自定义依赖、自定义事件、自定义触发器。

Keeper 基于 Akka 和 DataHub 框架开发，文档待补全。源码地址为：<https://github.com/qross-io/Keeper>

## # [统一接口管理 OneApi](/oneapi/overview.md)

**OneApi** 是 PQL 的另一种应用，即后端接口由 PQL 而不是 Java 或者 MyBatis 来开发。OneApi 可以极大的提高接口开发的效率，在接口中只需要编写核心逻辑，不再需要各种 Java 类。OneApi 运行在 Spring Boot 环境下。

* 在 Spring Boot 项目中，[Controller 只需要配置一次](/oneapi/controller.md)。
* OneApi 的接口可以写在 Spring Boot 项目的文件中，与项目一起受源代码管理。
* OneApi 的接口不需要会 Java 即可进行接口开发。
* OneApi 提供基于[静态 Token 和动态 Token 的权限验证](/oneapi/token.md)。
* 通过 PQL，可以输出任意 Json 格式的数据接口。
* 通过 PQL，可以方便的进行列转行等常用操作，满足前端开发需求。
* 通过 PQL，可快速访问任意关系型数据库和其他数据源如 Redis 等。
* 通过 PQL，可快速将输出各种文件并实现文件下载功能。

OneApi 的页面管理功能正在开发中，可更方便直观的管理项目和接口。示例项目地址为：<https://github.com/qross-io/OneApi>

## # [数据管理平台 Master](/master/overview.md)

**Qross Master** 是一个适用于数据部门或数据开发的综合管理平台，含数据管理、调度管理、接口管理、文档管理等功能。Master 的很多功能正在不断增加中。

## # [数据开发框架 DataHub](/datahub/overview.md)

**DataHub** 是一个简单易用的数据处理框架，整合了多个资源库，PQL 和 Keeper 都是基于 DataHub 框架开发。文档正在逐渐补全。

## # [模板引擎 Voyager](/voyager/overview.md)

**Voyager** 同样是基于 Spring Boot 的模板引擎，基础功能与 Freemarker 和 Thymeleaf 类似，但拥有更强大的功能和更简单、直接、高效的编码方式。

* 不需要再向 Model 中添加数据，Controller 里不需要有任何逻辑
* 所有逻辑都在前端实现，像 **Jsp** 一样，但嵌入 HTML 中的不是 Java 而是 PQL
* 不需要再记各种语法规则，直接通过 SQL 在页面上查询数据
* 通过 PQL 的条件和循环在页面上控制逻辑
* 支持[母版页](/voyager/master.md)
* 不仅支持 HTML 格式，还支持 [Markdown 格式](/voyager/markdown.md)
* 支持[国际化](/voyager/language.md)
* 支持邮件模板

## # [root.js 前端组件库](/root.js/overview.md)

**root.js** 是一个简单易用的前端组件库，包含很多个自定义组件用于实现各种交互功能，Qross 系统前端页面使用 root.js 库进行开发。

---
技术支持

官方网站 [www.qross.cn](http://www.qross.cn)  
联系作者 <wu@qross.io>