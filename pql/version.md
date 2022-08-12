# 版本和更新

作者会一个月发布一个稳定版本。PQL 的最新稳定版本为 **v2.5.0**，发布时间 **2022-5-31**。

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

