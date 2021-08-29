# 全局函数 - 字符串操作

PQL 全局函数提供了与数据库 SQL 中相应的字符串操作功能。当前版本支持的字符串函数按名称排序如下：

* **`@CHARINDEX(strToFind, strToSearch)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。@CHARINDEX('o', 'hello')`的结果是`5``。
* **`@CONCAT(str1, str2, ...)`** 连接两个或多个字符串，`@CONCAT('hel', 'lo', ' ', 'world')`返回`'hello world'`。
* **`@INSTR(strToSearch, strToFind)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@INSTR('world', 'r')`的结果是`3`。
* **`@LEFT(str, n)`** 从左向右截取`str`的`n`个字符串。
* **`@LEN(str)`** 或 `@LENGTH(str)` 返回字符串的字符长度。
* **`@LOWER(str)`** 将字母串所有英文字母转变成小写。
* **`@LPAD(str, length, padstr)`** 如果字符串`str`的长度不到`length`，则用重复的`padstr`在左面补全。如`@LPAD('5', 4, '0')`结果是`0005`。
* **`@LTRIM(str)`** 去掉字符串左面的空白字符。
* **`@POSITION(strToFind, strToSearch)`** 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@POSITION('o', 'world')`的结果是`2`，效果同`@CHARINDEX`。
* **`@REPEAT(str, n)`** 将字符串`str`重复`n`次，生成新的字符串。如`@REPEAC('ok', 3)`得到`okokok`。
* **`@REPLACE(str, old, replacement)`** 将字符串中的指定字符串`old`换成新字符串`replacement`，`REPLACE('hello-world', '-w', 'W')`结果是`helloWorld`。
* **`@REVERSE(str)`** 将字符串`str`进行反转，如`@REVERSE('ok')`得到`ko`。
* **`@RIGHT(str, n)`** 从右向左截取`str`的`n`个字符串。
* **`@RPAD(str, length, padstr)`** 如果字符串`str`的长度不到`length`，则用重复的`padstr`在右面补全。如`@RPAD('5', 4, '0')`结果是`5000`。
* **`@RTRIM(str)`** 去掉字符串右面的空白字符。
* **`@TRIM(str)`** 去掉字符串两面的空白字符。
* **`@UCASE(str)`** 将字母串所有英文字母转变成大写。
* **`@UPPER(str)`** 是`@UCASE`函数的同义词。

---
参考链接

* [Sharp 表达式 - 字符串操作](/sharp-text.md)
* [全局函数 FUNCTION](/pql/global-function.md)
* [全局函数 - 日期时间操作](/pql/function-datetime.md)
* [全局函数 - 数字操作](/pql/function-numeric.md)
* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)
