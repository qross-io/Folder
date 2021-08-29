# 全局函数 - 数字操作

PQL 全局函数提供了与数据库 SQL 中相应的数字操作功能。

* `@FLOOR(m)` 直接去掉小数部分，只保留整数。如`@FLOOR(1.75)`，结果是`1`。
* `@FLOOR(m, n)` 把小数`m`保留`n`位小数，多余位数直接去掉。如`@FLOOR(1.4875, 2)`，结果是`1.48`。
* `@RANDOM(n)` 返回`0`到`n`之间的随机整数。
* `@RANDOM(m, n)` 返回`m`到`n`之间的随机整数。
* `@ROUND(m)` 把小数转换成整数并四舍五入。如`@ROUND(1.75)`，结果是`2`。
* `@ROUND(m, n)` 把小数`m`保留`n`位小数，多余部分四舍五入。如`@ROUND(1.4875, 2)`，结果是`1.49`。

---
参考链接

* [Sharp 表达式 - 数字操作](/sharp-datetime.md)
* [全局函数 FUNCTION](/pql/global-function.md)
* [全局函数 - 字符串操作](/pql/function-text.md)
* [全局函数 - 日期时间操作](/pql/function-datetime.md)
* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)
