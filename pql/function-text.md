# 全局函数 - 字符串操作

PQL 全局函数提供了与数据库 SQL 中相应的字符串操作功能。当前版本支持的字符串函数按名称排序如下：

* `@CHARINDEX(strToFind, strToSearch)` 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。@CHARINDEX('o', 'hello')`的结果是`5``。
* `@CONCAT(str1, str2, ...)` 连接两个或多个字符串，`@CONCAT('hel', 'lo', ' ', 'world')`返回`'hello world'`。
* `@INSTR(strToSearch, strToFind)` 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@INSTR('world', 'r')`的结果是`3`。
* `@LEFT(str, n)` 从左向右截取`str`的`n`个字符串。
* `@LEN(str)` 或 `@LENGTH(str)` 返回字符串的字符长度。
* `@LOWER(str)` 将字母串所有英文字母转变成小写。
* `@LPAD(str, length, padstr)` 如果字符串`str`的长度不到`length`，则用重复的`padstr`在左面补全。如`@LPAD('5', 4, '0')`结果是`0005`。
* `@LTRIM(str)` 去掉字符串左面的空白字符。
* `@POSITION(strToFind, strToSearch)` 在字符串`strToSearch`中查找字符串`strToFind`的位置，索引从`1`开始，找不到返回`0`。`@POSITION('o', 'world')`的结果是`2`，效果同`@CHARINDEX`。
* `@REPEAT(str, n)` 将字符串`str`重复`n`次，生成新的字符串。如`@REPEAC('ok', 3)`得到`okokok`。
* `@REPLACE(str, old, replacement)` 将字符串中的指定字符串`old`换成新字符串`replacement`，`REPLACE('hello-world', '-w', 'W')`结果是`helloWorld`。
* `@REVERSE(str)` 将字符串`str`进行反转，如`@REVERSE('ok')`得到`ko`。
* `@RIGHT(str, n)` 从右向左截取`str`的`n`个字符串。
* `@RPAD(str, length, padstr)` 如果字符串`str`的长度不到`length`，则用重复的`padstr`在右面补全。如`@RPAD('5', 4, '0')`结果是`5000`。
* `@RTRIM(str)` 去掉字符串右面的空白字符。
* `@TRIM(str)` 去掉字符串两面的空白字符。
* `@UCASE(str)` 将字母串所有英文字母转变成大写。
* `@UPPER(str)` 是`@UCASE`函数的同义词。

与正则表达式相关的函数有：

* `@REGEXP_INSTR(str, regexp)` 查找指定正则表达式`regexp`在字符串`str`中的匹配位置，位置索引从`1`开始，如果不匹配则返回`0`。
* `@REGEXP_LIKE(str, regexp)` 判断字符串`str`是否与指定正则表达式`regexp`匹配，如果匹配返回`1`，否则返回`0`。
* `@REGEXP_REPLACE(str, regexp, replacement)` 替换字符串`str`中与与正则表达式`regexp`匹配的内容为`replacement`。
* `@REGEXP_SUBSTR(str, regexp)` 返回字符串`str`中与正则表达式`regexp`匹配的子字符串，如果不匹配则返回`null`。

区分大小写等匹配规则均写在正则表达式中，如`(?i)[a-z]+`中的`(?i)`表示忽略大小写，更多规则请参照[正则表达式语法](/root.js/regex.md)。

编码解码相关函数：

* `@BASE64_ENCODE(str, salt)` 使用 BASE 64 编码。参数`salt`是盐值，可以不设置。
* `@BASE64_DECODE(str, salt)` 解码 BASE 64。参数`salt`是盐值，可以不设置。

---
参考链接

* [Sharp 表达式 - 字符串操作](/sharp-text.md)
* [全局函数 FUNCTION](/pql/global-function.md)
* [全局函数 - 日期时间操作](/pql/function-datetime.md)
* [全局函数 - 数字操作](/pql/function-numeric.md)
* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)
