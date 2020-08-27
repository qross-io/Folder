# Sharp表达式 - 字符串操作
Sharp表达式为处理字符串提供了大量丰富且灵活的操作方法。下面按Link的用途分组介绍。

### 字符串截取和拆分
* **`CHAR AT n`** 返回字符串索引位置n的字符，索引从`1`开始。`'hello' CHAR AT 2`结果为`'e'`
* **`DROP n`** 或 **`DROP LEFT n`**  从左向右删除n个字符。`'hello' DROP 3`结果是`lo`
* **`DROP RIGHT n`** 从右向左删除n个字符。`'hello' DROP RIGHT 3`结果是`he`
* **`SPLIT 'regex'`** 将字符串按指定的分隔符拆分成数组，这里的参数是正则表达式字符串。如果分隔符中含有正则表达式的保留字符，必须加反斜杠转义，如 `'\|'`。同时也支持按正则表达式进行拆分。`'1,2,3' SPLIT ','`结果是`["1", "2", "3"]` 
* **`SPLIT 'delimiter' AND 'separater'`** 将字符串按两个分隔符分隔成一个数据行。`'a=1&b=2&c=3' SPLIT '&' AND '='`结果是`{ "a": "1", "b": "2", "c": "3"}`。这里的分隔符不支持正则表达式。
* **`SUBSTR n`** 取第m个字符及之后的所有字符，索引从`1`开始。`'hello' SUBSTR 4`结果是`'lo'`
* **`SUBSTR m TAKE n`** 从第m个字符开始取n个字符，索引从`1`开始。`'tomorrow' SUBSTR 4 TO 2`结果是`'or'`
* **`SUBSTR m TO n`** 取从第m个字符到第n个字符之间的所有字符，包括m但不包括n，索引从`1`开始。`'tomorrow' SUBSTRING 3 TO 5`结果是`mo`
* **`TAKE AFTER 'str'`** 或 **`TAKE AFTER FIRST 'str'`** 取第一个字符串`str`之后的所有字符。`'tomorrow' TAKE AFTER 'o'`结果是`'morrow'`
* **`TAKE AFTER LAST 'str'`** 取最后一个字符串`str`之后的所有字符。`'tomorrow' TAKE AFTER LAST 'o'`结果是`'w'`
* **`TAKE BEFORE 'str'`** 或 **`TAKE BEFORE FIRST 'str'`**  取第一个字符串`str`之前的所有字符。`'tomorrow' TAKE BEFORE 'mor'`结果是`'to'`
* **`TAKE BEFORE LAST 'str'`** 取最后一个字符串`str`之前的所有字符。`'tomorrow' TAKE BEFORE LAST 'o'`结果是`'w'`
* **`TAKE BETWEEN 'str1' AND 'str2'`** 取第一个字符串`str1`和最后一个字符串`str2`之间的所有字符。`'tomorrow' TAKE BETWEEN 'o' AND 'o'`结果是`'morr'`
* **`TAKE n`** 或 **`TAKE LEFT n`** 从左向右取n个字符。`'hello' TAKE 2`结果是`'he'`
* **`TAKE RIGHT n`** 从右向左取n个字符。 `'hello' TAKE RIGHT 2`结果是`'lo'`
* **`TRIM`** 去掉字符串两边的空白字符，空白字符包含空格`' '`、制表符`'\t'`、回车符`'\r'`和换行符`'\n'`。
* **`TRIM 'a'`** 去掉字符串两边的所有子字符串`a`，如果字符串两边有空白字符，那么先去掉空白字符再去掉子字符串`a`。`'#hello#' TRIM '#'`结果为`'hello'`
* **`TRIM 'a' AND 'b'`** 去掉字符串左边的字符串`a`和右面的字符串`b`。`'[hello]' TRIM '[' AND ']'`结果为`'hello'`
* **`TRIM LEFT 'a'`** 去掉字符串左边的字符串`a`，如果左边有空白字符，则先去掉空白字符。`'#hello#' TRIM LEFT '#'`结果为`'hello#'`
* **`TRIM RIGHT 'b'`** 去掉字符串右边的字符串`b`，如果右边有空白字符，则先去掉空白字符。`'#hello#' TRIM RIGHT '#'`结果为`'#hello'`

### 字符串替换
* **`REPLACE 'str1' TO 'str2'** 将字符串中的所有子字符串'str1'替换为字符串'str2'。`'tomorrow' REPLACE 'o' TO 'e'`结果为`'temerrew'`
* **`REPLACE ALL 'regex' TO 'str'`** 按正则表达式`regex`查找匹配并将所有匹配替换成字符串`str`。`'Hello World' REPLACE ALL 'A-Z' TO ''`结果是`'ello orld'`
* **`REPLACE FIRST 'str1' TO 'str2'`** 将字符串中的第一个子字符串`str``替换为字符串`str2`。`'tomorrow' REPLACE FIRST 'o' TO 'e'`结果为`'temorrow'`
* **`REPLACE LAST 'str1' TO 'str2'`** 将字符串中的最后一个子字符`str1`替换为字符串`str2`。`'tomorrow' REPLACE LAST 'o' TO 'e'`结果为`'tomorrew'`

#### 字符串查找和匹配
* **`INDEX OF 'str'`**  在字符串中从前向后查找子字符串`str`所在的位置，索引从`1`开始，找不到返回`-1`。`'tomorrow' INDEX OF 'o'`结果为`2`
* **`LAST INDEX OF 'str'`** 在字符串中从后向前查找子字符`str`所在的位置，索引从`1`开始，找不到返回`-1`。`'tomorrow' LAST INDEX OF 'o'`结果为`7`
* **`LIKE '%a%'`** 规则同SQL中的`LIKE`，判断一个字符串是否与给定的表达式相似，支持通配符`%`和`?`。`'hello' LIKE '%e%'`结果是`true`
* **`NOT LIKE '%a%'`** 规则同SQL中的`LIKE`，判断一个字符串是否与给定的表达式不相似，支持通配符`%`和`?`。`'hello' NOT LIKE '%e%'`结果是`false`
* **`MATCHES 'regex'`** 字符串是否匹配正则表达式`regex`。`'Hello' MATCHES "(?i)^h"`结果是`true`

### 字符串扩展
* **`CONCAT 'str'`** 连接两个字符串，效果同字符串连接符号`+`，如`'a' + 'b'`，但`CONCAT`的优先级比加号连接低，`'a' CONCAT 'b' + 'c'`中先执行`'b' + 'c'`
* **`QUOTE`** 无参数，将字符串用单引号引起来。`"hello" QUOTE`结果为`"'hello'"`
* **`QUOTE 'a'`** 将字符串用字符串`a`引起来。`'hello' QUOTE '"'`结果为`'"hello"'`，效果同`BRACKET 'str'`
* **`RANDOM n`** 从字符串随机取`n`个字符，组成新的字符串，`n`大于等于`1`且`n`可以大于字符串的长度。这个Link按给定的字符列表生成随机字符串，如随机密码等。如`'abcdefghijklmnopqrstuvwxyz' RANDOM 6`结果可能是`hmiqez`
* **`REPEAT n`** 重复字符串n次。`'ab' REPEAT 3`结果为`'ababab'`

### 字符串判断
* **`BRACKET 'str'`** 将字符串用字符串`str`包围起来。`'hello' BRACKET '#'` 结果为 `'#hello#'`
* **`BRACKET 'str1' AND 'str2'`** 将字符串用字符`str1`和字符串`str2`包围起来。`'hello' BARCKET '[' AND ']'`结果为`'[hello]'`
* **`BRACKETS WITH 'str'`** 判断字符串是否使用字符串`str`包围。`'#hello#' BRACKETS WITH '#'`结果为`true` 
* **`BRACKETS WITH 'a' AND 'b'`** 判断字符串是否使用字符串`a`和`b`包围。`'hello' BRACKETS WITH '[' AND ']'`结果为`false`
* **`CONTAINS 'str'`** 判断字符串是否包含子字符串'str'。`'hello' CONTAINS 'e'`结果是`true`
* **`CONTAINS IGNORE CASE 'str'`** 判断字符串是否包含子字符串'str'，忽略大小写。`'Hello' CONTAINS 'h'`结果是`true`
* **`ENDS WITH 'str'`** 判断字符串是否以字符串`str`结束。`'hello' ENDS WITH 'e'`结果是`false`
* **`ENDS IGNORE CASE WITH 'str'`** 判断字符串是否以字符串`str`结束，忽略大小写。`'hello' ENDS IGNORE CASE WITH 'LO'`结果是`true`
* **`EQUALS 'str'`** 判断字符串是否和字符串`str`相等，效果同`==`，但执行优先级高于`==`。这个Link适用于所有数据类型，尽量让比较的数据类型一致。`'1' EQUALS 1`结果是`true`
* **`EQUALS IGNORE CASE 'str'`** 判断字符串是否和字符串`str`相等，忽略大小写。`'hello' EQUALS IGNORE CASE 'Hello'`结果是`true`
* **`CAPITALIZE`** 将字符串中的第一个字母大写。`'hello world' CAPITALIZE`结果为`'Hello world'`
* **`STARTS WITH 'str'`** 判断字符串是否以字符串`str`开始。`'hello' STARTS WITH 'H'`结果是`false`
* **`STARTS IGNORE CASE WITH 'str'`** 判断字符串是否以字符串`str`开始，忽略大小写。`'hello' STARTS IGNORE CASE WITH 'H'`结果是`true`

### 字符串转换和其他操作
* **`LENGTH`** 返回字符串的长度。`'hello' LENGTH`结果是`5`。
* **`TO INT`** 或 **`TO INTEGER`** 将字符串尝试转变成整数，转换失败抛出异常。
* **`TO INT value`** 或 **`TO INTEGER value`** 将字符串尝试转变成整数，转换失败则返回默认值`value`。
* **`TO DECIMAL`** 将字符串尝试转变成小数，转换失败能抛出异常。
* **`TO DECIMAL value`** 将字符串尝试转变成小数，转换失败能返回默认值`value`。
* **`TO LOWER`** 或 **`TO LOWER CASE`** 将字符串中的所有字母转化为小写。`'Hello' TO LOWER`结果为`'hello'`
* **`TO UPPER`** 或 **`TO UPPER CASE`** 将字符串中的所有字母转化为大写。`'Hello' TO UPPER`结果为`'HELLO'`
* **`TICK BY 'datetime'`** 根据指定的时间`datetime`获取Cron表达式的下一次匹配。`'0 0 * * * ? *' TICK BY '2020-08-08 12:28:35'`结果是`'2020-08-08 13:00:00'`
* **`TO BASE64`** 将字符串编码为BASE64字符串。`'123456' TO BASE64`结果是`'MTIzNDU2'`
* **`DECODE BASE64`** 解码BASE64字符串，是`TO BASE64`的逆运算。
* **`TO MD5`** 将字符串编码为MD5字符串。`'123456' TO MD5`结果是`'e10adc3949ba59abbe56e057f20f883e'`
* **`URL ENCODE 'charset'`** 对字符串进行URL编码，`charset`参数可以省略，默认`UTF-8`。
* **`URL DECODE 'charset'`** 对已经进行过URL编码的字符串进行解码，`charset`参数可以省略，默认`UTF-8`。  

---
参考链接

* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)
* [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)
* [条件表达式](/pql/condition.md)