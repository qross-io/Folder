# 条件表达式

条件表达式用于判断条件是否成立，以执行相应的语句或功能。条件表达式应用到 IF 语句、IF 短语句、CASE 语句、CASE 短语句、WHILE 语句、EXIT 语句和 CONTINUE 语句中。条件表达式由一个或多个基本条件式构成，基本条件式之间再通过逻辑运算最后得到最终的结果。条件表达式的结果为布尔类型，只有真`true`或假`false`两个值。

## 基本条件式

基本条件表达式用于比较两个变量、值或表达式是否满足给定的操作符条件，如果是表达式，会计算完表达式的值再进行比较。基本操作符包括：

* 相等 `=`或`==` ，如 `$s == 'hello'`。字符串比较时忽略大小写，例如`'a' == 'A'`，结果是`true`。
* 不相等 `!=`或`<>`，如 `$i <> 1`，字符串比较忽略大小写。
* 绝对相等 `===`，字符串比较时区分大小写。例如`'a' === 'A'`，结果是`false`。
* 绝对不相等 `!==`，字符串比较时区分大小写。
* 大于 `>`，如`$x > 5`
* 大于等于 `>=`，如`$d >= '2020-08-01'`
* 小于 `<`，如`$y < 10`
* 小于等于 `<=`，如`$x <= 8 POW 3`

“相等”和“不相等”可以比较任何类型的值，但一般用于基本数据类型的比较。如果是复合类型，会先转成字符串再进行比较。“大于”、“大于等于”、“小于”、“小于等于”，一般用于数字类型比较，也可用于日期 时间比较，如果是字符串，则会使用排序规则进行比较，如`'A' < 'Z'`为`true`。

## 逻辑运算 AND/OR/NOT

逻辑运算用于两个基本表达式结果之间的进一步运算，只有三个运行符：与`AND`、或`OR`和非`NOT`。逻辑运算只接受布尔类型的值。

* 与`AND`，两端均为`true`时计算结果才为`true`，其他情况为`false`
* 或`OR`，两端有一端为`true`时计算结果就为`true`，两端都为`false`时计算结果才为`false`
* 非`NOT`为单值运算符，取输入值的反值，输入值为`true`，计算结果为`false`; 输入值为`false`，计算结果为`true`。NOT在PQL中使用非常少，一般会用基本运算符代替。

```sql
$a > 1 AND $a < 10
$a < 0 OR $a > 10
NOT $a > 3 
```

逻辑运行符优先级：在同一个逻辑运算表达式中，如果表达式没有被小括号包含，且同时拥有 3 个运算符，则计算顺序是先`NOT`再`AND`最后`OR`。不过为了增加程序的可读性，建议用小括号对复杂的逻辑运算进行分组，分组还可以多重嵌套。

```sql
($str == 'A' OR $str == 'B') AND $time >= @NOW
```


除了基本运行符之外，PQL 还提供了跟 SQL 语句一样的扩展运算符`IN`、`EXIST`、`LIKE`和`IS`，下面分别介绍。

## 判断元素是否在一个集合中 IN

与 SQL 中的`IN`表达式意义相同，判断指定变量的值或元素是否在一个集合（确切的说是数组）中。例如：

```sql
IF @ROLE IN ('leader', 'worker') THEN
    PRINT $username;
END IF;

-- IN 的其他形式（省略了IF语句）
$rate NOT IN ('A', 'B', 'C')
$id IN (SELECT id FROM students WHERE score>75)
$number IN [1, 2, 3, 4, 5]
```

IN 表达式判断的内容如果是表达式，必须用小括号括起来。

## 判断一个查询语句是否有返回值 EXISTS

与 SQL 中`EXISTS`表达式的意义相同，但是除了 SELECT 语句之外，支持其他的查询语句。

```sql
IF EXISTS (SELECT id FROM students WHERE score=100) THEN
    PRINT 100;
END IF;

-- EXISTS 的其他形式（省略了IF语句THEN后面的内容）
IF NOT EXISTS (SELECT id FROM students WHERE score<60) THEN
IF EXISTS (REDIS KEYS *) THEN
```

EXISTS 表达式只支持有结果的查询语句，且需要用小括号括起来。EXISTS 还支持文件和目录是否存在的判断，见 [FILE 语句](/pql/file.md)和 [DIR 语句](/pql/dir.md)。

## LIKE 关键词

与 SQL 中的`LIKE`表达式意义相同，用于判断字符串是否模糊匹配另一个指定字符串。

```sql
IF $str LIKE '%hello%' THEN
    PRINT $str;
END IF;

IF $name NOT LIKE 'T??' THEN
    PRINT $name;
END IF;
```

在 PQL 中，`LIKE`用得比较少，Sharp 表达式中提供丰富的字符串的操作方法，见下面的 “Sharp 中的布尔表达式”。

## NULL/UNDEFINED/EMPTY 关键词

在介绍 [PQL 变量](/pql/variable.md)时，同时介绍了变量的几种状态，这里补充一下变量状态判断的内容。

```sql
IF $n IS NULL THEN
    PRINT '$n is null.';
END IF;

-- 更多示例（省略了IF语句）
$m IS NOT NULL
$row.name IS UNDEFINED -- 判断数据行类型是否有某个字段
$y IS NOT UNDEFINED
$y IS DEFINED -- 上面语句的简写版
$list IS EMPTY -- 判断列表是否为空
$str IS NOT EMPTY -- 判断字符串是否为空，不过一般写成 $str != ''
```

* 判断是否成立用`IS`或`IS NOT`，而不是用`==`或`!=`。
* `NULL`表未变量已声明未赋值状态，或者确实赋了一个`NULL`值。
* `UNDEFINED`表示变量未声明未赋值状态。
* `EMPTY`表示集合类型或字符串是否为空。
* 特别注意，因为开发人员有时很容易混淆`NULL`和`UNDEFINED`的分别代表的意义，所以在 PQL 中这两个值是相等的，即`NULL == UNDEFINED`成立。如果非要区分出这两个值，可以用绝对相等 `===` 和绝对不相等 `!==`进行判断，即`NULL === UNDEFINED`不成立。

## 数据类型判断

虽然 PQL 会自动推断数据类型，但是有时仍需要对数据类型进行判断以进行相应的操作。数据类型判断需要使用数据类型关键词。目前支持的数据类型关键词如下：

* `TABLE`或`DATABLE`，表示数据表。
* `ROW`或`DATAROW`，表示数据行。
* `ARRAY`或`LIST`，表示数组。
* `TEXT`或`STRING`，表示字符串。
* `INTEGER`或`INT`，表示整数。
* `DECIMAL`或`NUMERIC`，表示小数。
* `BOOLEAN`或`BOOL`，表示布尔。
* `DATETIME`，表示日期时间。

数据类型也用`IS`或`IS NOT`进行判断。

```sql
IF $s IS DATETIME THEN
    PRINT ${ $s TO EPOCH };
END IF;
```

## Sharp 表达式中的布尔操作

在 Sharp 表达式中，为各数据类型提供了丰富的处理方法，其中也包括了一些判断操作，上述的 LIKE 和 NOT LIKE 就是通过 Sharp 表达式的逻辑实现的。每个 Sharp 表达式基本式由“数据+连接+参数”构成，其中参数有时可省略。这里只介绍与判断有关的表达式，其他的在[Sharp表达式](/pql/sharp.md)中介绍。

* `datetime BEFORE other_datetime` 判断一个时间是否早于另一个时间，可以用`<`代替
* `datetime BEFORE OR EQUALS` 判断一个时间是否早于或等于另一个时间，可以用`<=`代替
* `datetime AFTER other_datetime` 判断一个时间是否晚于另一个时间，可以用`>`代替
* `datetime AFTER OR EQUALS` 判断一个时间是否早于或等于另一个时间，可以用`>=`代替
* `datetime MATCHES CRON cron_exp` 判断一个时间是否匹配指定的Cron表达式
* `value EQUALS other_value` 判断一个值是否等于另一个值，可以用`==`代替。
* `str1 EQUALS IGNORE CASE str2` 判断两个字符串是否相等（忽略大小写）
* `str1 STARTS WITH str2` 判断字符串str1是否以字符串str2开头
* `str1 STARTS WITH IGNORE CASE str2` 判断字符串str1是否以字符串str2开头，忽略大小写
* `str1 ENDS WITH str2` 判断字符串str1是否以字符串str2结束 
* `str1 ENDS WITH IGNORE CASE str2` 判断字符串str1是否以字符串str2结束，忽略大小写
* `str1 CONTAINS str2` 判断字符串str1是否包含字符串str2 
* `str1 CONTAINS IGNORE CASE str2` 判断字符串str1是否包含字符串str2，忽略大小写
* `str BRACKETS WITH str1 AND str2` 判断字符串str是否以str1开头并以str2结尾
* `str MATCHES regex_or_str` 判断字符串是否匹配正则表达式regex
* `regex TEST str` 判断正则表达式是否匹配字符串

更多 Sharp 布尔表达式正在扩展。由于 Sharp 表达式的存在，函数的作用基本被忽略。PQL 到当前版本为止，仍缺少对函数的支持。在未来的某个版本中（预计`0.7.0`之前），PQL 会把函数添加进来，到时条件表达式会增加布尔函数的内容。

---
参考链接

* [条件控制 IF](/pql/if.md)
* [条件控制 CASE](/pql/case.md)
* [条件循环 WHILE](/pql/while.md)
* [退出循环 EXIT](/pql/exit.md)
* [继续下一次循环 CONTINUE](/pql/continue.md)
* [Sharp 表达式](/pql/sharp.md)
* [数据类型](/pql/datatype.md)
* [文件操作 FILE](/pql/file.md)
* [文件夹操作 DIR](/pql/dir.md)