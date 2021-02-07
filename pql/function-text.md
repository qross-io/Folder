# 全局函数 - 字符串操作

PQL 全局函数提供了与数据库 SQL 中相应的字符串操作功能。当前版本支持的字符串函数按名称排序如下：

* **`@CHARINDEX(strToFind, strToSearch)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。@CHARINDEX('o', 'hello')`的结果是`5``。
* **`@CONCAT(str1, str2, ...)`** 连接两个或多个字符串，`@CONCAT('hel', 'lo', ' ', 'world')`返回`'hello world'`。
* **`@INSTR(strToSearch, strToFind)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@INSTR('world', 'r')`的结果是`3`。
* **`@LEFT(str, n)`** 从左向右截取`str`的`n`个字符串。
* **`@POSITION(strToFind, strToSearch)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@POSITION('o', 'world')`的结果是`2`，效果同`@CHARINDEX`。
* **`@REPLACE(str, old, replacement)`** 将字符串中的指定字符串`old`换成新字符串`replacement`，`REPLACE('hello-world', '-w', 'W')`结果是`helloWorld`。
* **`@RIGHT(str, n)`** 从右向左截取`str`的`n`个字符串。

---
参考链接

* [全局函数 FUNCTION](/pql/global-function.md)
* [全局函数 - 数字操作](/pql/function-numeric.md)
* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)
