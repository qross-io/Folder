# Sharp表达式 - 日期时间操作
在开发中时间类型应用非常多，PQL在Sharp表达式中实现了各种各样的时间处理方法，下面均以变量`$datetime`为例进行分组介绍。
```sql
SET $datetime := '2019-04-15 13:27:05'; -- 星期一
PRINT ${ $datetime FORMAT 'yyyyMMddHHmmss' }; -- 值为 20190415132705
```
### 日期时间格式化
* **`FORMAT 'format'`** 按指定的格式对日期时间进行格式化，返回字符串。下面说明一下格式中各个字母的意义：
	+ **`y`** 表示年，2位返回`19`，4位返回`2019`
	+ **`M`** 表示月，1位返回`4`，2位返回`04`，3位返回`Apr`，4位返回 `April`。中文环境下返回中文，如`四月`。
	+ **`d`** 表示天，1位和2位都返回`15`，1位时小于`10`才返回1位数字
	+ **`H`** 表示24小时制小时，1位和2位都返回`13`，1位时小于`10`返回1位数字
	+ **`h`** 表示12小时制小时，1位返回`1`，2位返回`01`
	+ **`m`** 表示分钟，1位和2位都返回`27`，1位时分钟小于`10`时返回1位数字。
	+ **`s`** 表示秒，1位返回`5`，两位返回`05`
	+ **`S`** 表示毫秒，一般为3位
	+ **`e`** 表示星期，1位返回`1`，2位返回`01`，3位返回`Mon`，4位返回`Monday`。中文环境下返回中文，如`星期一`。
	+ **`a`** 表示上下午，返回`AM`或`PM`。中文环境下返回`上午`或`下午`。

### 获取日期时间的某一部分
以下Link的返回值为整数。
* **`GET YEAR`** 返回年份，结果为`2019`
* **`GET MONTH`** 返回月份，结果为`4`
* **`GET DAY`** 返回日期，结果为`15`
* **`GET HOUR`** 返回小时，结果为`13`
* **`GET MINUTE`** 返回分钟，结果为`27`
* **`GET SECOND`** 返回秒，结果为 `5`
* **`GET MILLI`** 返回毫秒，秒后3位
* **`GET MICRO`** 返回微秒，秒后6位
* **`GET NANO`** 返回纳秒，秒后9位
* **`GET WEEK`** 返回星期，结果为`1`。星期二返回`2`，星期日返回`7`
* **`GET WEEK NAME`** 返回三位的星期名称字符串，同`FORMAT 'eee'`。

### 设置日期时间的某一部分
以下Link的参数均为整数，返回值均为新的日期时间。
* **`SET YEAR  n`** 设置年份，接受4位整数。
* **`SET MONTH n`** 设置月份，接受`1`到`12`之间的整数。
* **`SET DAY n`** 设置日期，接受`1`到`31`之间的整数，但不能设置不存在的日期，比如`2月30日`。
* **`SET HOUR n`** 设置小时，接受`0`到`23`之间的整数。
* **`SET MINUTE  n`** 设置分钟，接受`0`到`59`之间的整数。
* **`SET SECOND  n`** 设置秒，接受`0`到`59`之间的整数。
* **`SET MILLI  n`**  毫秒，秒后3位
* **`SET MICRO  n`** 微秒，秒后6位
* **`SET NANO  n`**  纳秒，秒后9位
* **`SET WEEK  n`** 接受`1`到`7`之间的整数，分别代表星期一到星期日。

### 增加或减少日期时间的某一单位
以下Link的参数均为整数，且均支持负值，返回值均为新的日期时间。
* **`PLUS n`** 在当前日期时间上增加n毫秒。
* **`PLUS n [UNIT]`** 在当前日期时间上增加n个时间单位。`[UNIT]`代表时间单位，可选值见下面的说明。
* **`PLUS YEARS n`** ** 在当前日期时间上增加n年。
* **`PLUS MONTHS  n`** 在当前日期时间上增加n月。
* **`PLUS DAYS  n`** 在当前日期时间上增加n天。
* **`PLUS HOURS  n`** 在当前日期时间上增加n小时。
* **`PLUS MINUTES  n`** 在当前日期时间上增加n分钟。
* **`PLUS SECONDS  n`** 在当前日期时间上增加n秒。
* **`PLUS MILLIS  n`** 在当前日期时间上增加n毫秒。
* **`PLUS MICROS  n`** 在当前日期时间上增加n微秒。
* **`PLUS NANOS  n`** 在当前日期时间上增加n纳秒。

* **`MINUS n`** 在当前日期时间上减去n毫秒。
* **`MINUS n [UNIT]`** 在当前日期时间上增加n个时间单位。`[UNIT]`代表时间单位，可选值见下面的说明。
* **`MINUS YEARS  n`** 在当前日期时间上减去n年。
* **`MINUS MONTHS n`** 在当前日期时间上减去n月。
* **`MINUS DAYS  n`** 在当前日期时间上减去n天。
* **`MINUS HOURS  n`** 在当前日期时间上减去n小时。
* **`MINUS MINUTES  n`** 在当前日期时间上减去n分钟。
* **`MINUS SECONDS  n`** 在当前日期时间上减去n秒。
* **`MINUS MILLIS  n`** 在当前日期时间上减去n毫秒。
* **`MINUS MICROS  n`** 在当前日期时间上减去n微秒。
* **`MINUS NANOS n`** 在当前日期时间上减去n纳秒。

上面有两个Link用到了时间单位，时间单位也属于Sharp表达式的Link，作用是将整数的时间单位转成毫秒值，如`$datetime PLUS 20 MINUTES`或`$datetime MINUS 1 HOUR`。所有支持的单位如下：
* **`YEAR`** 或 **`YEARS`** 年
* **`MONTH`** 或 **`MONTHS`** 月
* **`DAY`** 或 **`DAYS`** 天
* **`HOUR`** 或 **`HOURS`** 小时
* **`MINUTE`** 或 **`MINUTES`** 分钟
* **`SECOND`** 或 **`SECONDS`** 秒
* **`MILLI`** 或 **`MILLIS`** 毫秒
* **`MICRO`** 或 **`MICROS`** 微秒
* **`NANO`** 或 **`NANOS`** 纳秒


下例中4个Sharp表达式的结果相同，可根据个人习惯选择不同的操作。
```sql
$datetime PLUS DAYS -1;
$datetime PLUS -1 DAY;
$datetime MINUS DAYS 1;
$datetime MINUS 1 DAY;
```
#### 快速编辑日期时间
除上面提供了PLUS和MINUS外，Sharp表达式中还提供了额外的设置日期时间的方法。
* **`EXPRESS 'expression'`** 通过时间表达式快速设置日期时间。举个例子，使用上面介绍的方法将日期设置到本月最后一天的`0`点`0`分`0`秒：
    ```sql
    $datetime PLUS MONTHS 1 SET DAY 1 MINUS DAYS 1 SET HOUR 0 SET MINUTE 0 SET SECOND 0 SET NANO 0
    ```
    用`EXPRESS`可以简写为：
    ```sql
    $datetime EXPRESS 'MONTH+1#DAY=1#DAY-1#HOUR=0#MINUTE=0#SECOND=0#NANO=0'
    或
    $datetime EXPRESS 'DAY=L#HOUR=0#MINUTE=0#SECOND=0#NANO=0'
    ```
    + `EXPRESS`由一个或多个时间表达式构成，时间表达式之间用井号`#`隔开。
    + 每个时间表达式由“时间单位”+“操作符”+“值”构成。
    + 支持的时间单位有：`YEAR`,`MONTH`,`DAY`,`WEEK`,`MINUTE`,`SECOND`,`MILLI`,`MICRO`,`NANO`。时间单位不区分大小写。
    + 支持的操作符有：`=`表示设置，`+`表示增加，`-`表示减去。
    + 值部分除了支持各时间单位允许的整数值。
    + 设置`DAY`时支持`L`，表示最后一天。
    + 设置`WEEK`时支持三位的星期名称：Mon,Tue, Wed, Thu, Fri, Sat, Sun
* **`SET ZERO OF DAY`** 设置时间为当天的`0`点`0`分`0`秒。
* **`SET BEGINNING OF MONTH`** 设置时间为当月的每`1`天的`0`点`0`分`0`秒。

### 日期时间比较、转换和其他
日期时间比较和数字的比较类似，一般比较两个时间的早晚或者差值。
* **`AFTER 'otherTime'`** 判断指定的日期时间是否晚于另一个日期时间，基本等效于`>`。
* **`AFTER OR EQUALS 'otherTime'`** 判断指定的日期时间是否晚于或等于另一个日期时间，基本等效于`>=`。
* **`BEFORE 'otherTime'` 判断指定的日期时间是否早于另一个日期时间，基本等效于`<`。
* **`BEFORE OR EQUALS 'otherTime'` 判断指定的日期时间是否早于或等于另一个日期时间，基本等效于`<=`。
* **`EQUALS 'otherTime'`** 判断指定的日期时间是否等于另一个日期时间，基本等效于`=`或`==`。

    上面几个Link与比较操作符的区别为：比较操作符会优先转成数字做比较，`AFTER`和`BEFORE`会将两端的值转成日期时间再做比较；`EQUALS`如果两端有一个是日期时间，则两端都转成日期时间做比较，否则都转成字符串做比较。举个例子：`2020-08-17 12:00:00`转成纪元秒为`1597636800`，`1597636800 AFTER '2020-08-07 12:00:00'`结果为`true`，但是`1597636800 > '2020-08-07 12:00:00'`会报错。
* **`LATER 'otherTime'`** 计算指定日期时间晚于另一个日期时间多少毫秒，负数表示早于另一个日期时间。
* **`EARLIER 'oterTime'`** 计算指定日期时间早于另一个日期时间多少毫秒，负数表示晚于另一个日期时间。
* **`SPAN 'otherTime'`** 计算指定日期日期和另一个日期时间之间的毫秒差，返回值均大于等于`0`。日期时间差为毫秒数，一般会转化为狗日的读的天或小时等数字，如`$datetime SPAN $time2 TO DAYS`。
* **`TO EPOCH`** 将日期时间转化为纪元秒，10位整数。
* **`TO EPOCH SECOND`**  同上。
* **`TO EPOCH MILLI`**  将时间转化为纪元毫秒，13位整数。
* **`TO DATETIME`** 可以将10位纪元秒整数或13位纪元毫秒整数转化为日期时间，也可以尝试将其他类型的数据比如日期时间字符串转成日期时间。一般情况下会自动判断，自动判断不正确时可以使用这个Link。可转换格式见本章最后一节。
* **`TO DATETIME 'format'`** 将其他类型的值按给定格式转化为时间，一般情况下会自动判断，转换不了时可使用这个Link。如`'2020年8月4日' TO DATETIME 'yyyy年M月d日'`。
* **`MATCHES CRON 'cron_exp'`** 判断日期时间是否匹配指定的Cron表达式，经常会和`TICK BY`配合使用。

### 其他与日期时间相关的表达式
这些Link要编辑的数据都不是日期时间，最后一个是字符串，其他是整数。但这些Link都与日期时间有关。
* **`TO SECONDS`** 将毫秒数转化为秒，返回小数，一般与`SPAN`连用
* **`TO MINUTES`** 将毫秒数转化为分钟，返回小数，一般与`SPAN`连用
* **`TO HOURS`** 将毫秒数转化为小时，返回小数，一般与`SPAN`连用
* **`TO DAYS`** 将毫秒数转化为天，返回小数，一般与`SPAN`连用
* **`TO TIMESPAN 'units'`** 将毫秒数转化易读的字符串，一般与`SPAN`连用。如果不设置`units`参数，默认的单位为`'d,h,m,s,ms'`，返回值的格式为`'1d2h'`，最多只有两个时间单位。可以设置时间单位，如`'天,小时,分钟,秒,毫秒'`，前面的返回值就会变成`'1天2小时'`。
* **`TICK BY 'datetime'`** 根据指定的时间`datetime`获取Cron表达式的下一次匹配。`'0 0 * * * ? *' TICK BY '2020-08-08 12:28:35'`结果是`'2020-08-08 13:00:00'`


### 日期时间的自动转化规则
Sharp表达式在执行时会根据Link名称自动将要操作的值转成相应的类型，下面是可转成日期的格式列表。
* `'yyyyMMdd'` 如`'20200815'`转成`'2020-08-15 00:00:00'`
* `'HH:mm:ss'` 如`'16:31:25'`转成`'2020-08-17 16:31:25'`，即加上今天（2020年8月17日）的日期。
* `'yyyy-MM-dd'`
* `'yyyy/MM/dd'`
* `'dd/MM/yyyy'`
* `'yyyyMMddHHmm'`
* `'yyyyMMddHHmmss'`
* `'yyyyMMddHHmmss.S'` 含1位毫秒
* `'yyyyMMddHHmmss.SS'` 含2位毫秒
* `'yyyyMMddHHmmss.SSS'` 含3位毫秒
* `'yyyy-MM-dd HH:mm:ss'`
* `'yyyy-MM-dd HH:mm:ss.S'` 含1位毫秒
* `'yyyy-MM-dd HH:mm:ss.SS'` 含2位毫秒
* `'yyyy-MM-dd HH:mm:ss.SSS'` 含3位毫秒
* `10`位整数，按纪元秒转换
* `13`位整数，按纪元毫秒转换

当要使用的日期时间不在上述格式中时，可用`TO DATETIME 'format'`进行转换。


---
参考链接
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)
* [条件表达式](/pql/condition.md)