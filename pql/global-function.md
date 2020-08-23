# 全局系统函数
和任何数据库的SQL语言一样，PQL支持系统函数。系统函数即将在新版本中支持。
例如 `@REPLACE(string, char, replacement)`
与全局变量一样，系统函数使用`@`开头。
可以定义自己的系统函数，可以作用于整个系统。需要通过Qross Master管理工具定义。

---
参考链接
* [函数调用 CALL](/pql/call.md)
* [自定义函数 FUNCTION](/pql/function.md)