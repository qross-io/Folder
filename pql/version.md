# 版本和更新

不定期发布版本，当前最新版本 **4.3.0**，发布时间 **2024-3-6**。

## 2024 年累计更新

* [OneApi 应用](/oneapi/overview.md)性能优化：接口返回值示例功能关闭，流量统计功能弃用。(2024.3.6)
* [TRANS 语句](/pql/trans.md)及多个[数据表操作相关的 Sharp 表达式](/pql/sharp-table.md)如`WHERE`、`DELETE`、`SELECT`、`DISTINCT`当返回值为空时保留原表的数据结构。(2024.2.25)
* [CACHE 语句](/pql/cache.md)现在创建默认自增主键`_pk_id`。(2024.1.21)

## 2023 年累计更新

* 包名由`io.qross`改为`cn.qross`。(2023.12.31)
* 新增全局变量`@YEAR`, `@MONTH`, `@DAY`, `@HOUR`, `@MINUTE`, `@SECOND`, `@MILLISECOND`, `@MICROSECOND` 获取当前日期时间的各部分，均返回整数。(2023.12.23)
* [Sharp 表达式](/pql/sharp.md)中的 [SELECT](/pql/sharp-table.md) 现在支持五种常用聚合函数。（2023.12.22）
* [Sharp 表达式](/pql/sharp.md)新增 [DISTINCT](/pql/sharp-table.md) 操作，用于对指定列进行排重，并返回新的数据表。(2023.12.17)
* 新增 [SHIFT](/pql/trans.md)语句，用于对缓存数据表执行 SHARP 表达式。(2023.12.17)
* 新增 [TRANS](/pql/trans.md)语句，用于将数据表转换为指定的格式。(2023.12.17)
* 优化项：不启用 DEBUG 模式也提示未赋值的变量。(2023.11.10)
* PQL 函数嵌套逻辑 bug 修复，现在可以嵌套多个函数。(2023.9.2)
* PQL 全局函数增加`@PROPERTIES(key)`用于获取 properties 文件中的配置信息。(2023.9.2)
* [OneApi](/oneapi/overview.md) 的 token 现在支持 Header 和地址参数双重验证。(2023.5.26)
* [Voyager](/voyager/overview.md)静态站点和图片站点引用现在由处理`@`和`%`开始，变为`@/`和`%/`以防止冲突。(2023.2.16)
* Javascript 执行引擎修改为`Graalvm.js`，高版本 Java 中将移除`ScriptEngine` (2023.2.1)


## v2.6.0 (2022.6.30)

本月只有 1 项更新。

* 底层 JDBC 单例对象新增`setAsDefault`方法，`setup`方法修改名称为`refresh`。

## v2.5.0 (2022.6.1)

本月共 5 项更新。

* 新增[字符串函数](/pql/function-text.md)`@BASE64_ENCODE`和`@BASE64_DECODE`。
* Email 模板现在支持通过 PLACE DATA 为模板引擎传递参数变量，见 [SEND 语句](/pql/send.md)。
* 其他的性能优化和修复。

## v2.4.0 (2022.4.30)

本月共 9 项更新，没有新功能。

* [SEND 语句](/pql/send.md)中 PLACE 和 AT 语法的支持，使用 PLACE DATA 足够了。
* JDBC 数据源连接相关类的一些方法进行了优化。
* 其他的优化和 BUG 修复。

## 以前的版本更新

* [2021 年所有更新](/pql/history-2021.md)
* [v1.1.0 之前的版本更新](/pql/history.md)

---
参考链接

* [PQL 概览](/pql/overview.md)
* [OneApi 统一接口](/oneapi/overview.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [DataHub 数据开发框架](/datahub/overview.md)

