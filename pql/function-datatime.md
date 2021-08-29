# 全局函数 - 日期时间

目前 PQL 已经支持的日期时间操作函数如下：

* `@CURRENT_DATE()`或`@CURDATE()` 获取当前日期字符串，返回值格式`yyyy-MM-dd`，如`2021-08-29`。
* `@CURRENT_TIME()`或`@CURTIME()` 获取当前时间字符串，返回值格式`HH:mm:ss`，如`18:07:20`。
* `@CURRENT_TIMESTAMP()` 获取当前时间戳，与全局变量`@TIMESTAMP`结果一致。
* `@DATE_ADD(datetime, n, unit)`或`@ADDDATE(datetime, n, unit)`在指定日期时间`datetime`上增加`n`个单位`unit`时间，`unit`参数可选值为`SECOND|MINUTE|HOUR|DAY|MONTH|YEAR`，单数复数均可。例如`@DATE_ADD('2021-08-15 16:00:00', 2, DAYS)`结果是`2021-08-17 16:00:00`。
* `@DATE_SUB(datetime, n, unit)`或`@SUBDATE(datetime, n, unit)`在指定日期时间`datetime`上减去`n`个单位`unit`时间，`unit`参数可选值为`SECOND|MINUTE|HOUR|DAY|MONTH|YEAR`，单数复数均可。例如`@DATE_SUB('2021-08-15 16:00:00', 1, MONTH)`结果是`2021-07-15 16:00:00`。
* `@DATETIME_FORMAT(datetime, formatStyle)`或`@DATE_FORMAT(datetime, formatStyle)` 将指定日期时间转成需要的格式， `formatStyle`的参数比较多，见 [Sharp 表达式](/sharp-datetime.md)中的日期时间格式化。
* `@DAYNAME(datetime)`或`@WEEKNAME(datetime)` 获取指定日期时间的星期简称，根据语言环境返回值不同，如中文环境是`周日`，英文是`Sun`。
* `@DAYOFMONTH(datetime)`或`@DAY(datetime)` 获取指定日期时间是当月的第几天，取值`1`到`31`。
* `@DAYOFWEEK(datetime)`或`@WEEK(datetime)` 获取指定日期时间是星期几，星期一到星期日分别对应`1`到`7`。
* `@DAYOFYEAR(datetime)` 获取指定日期时间是当年的第几天，取值`1`到`366`。
* `@FROM_UNIXTIME(timestamp, formatStyle)` 将时间戳按指定的格式`formatStyle`转成日期时间，如果不指定`formatStyle`，则默认格式为`yyyy-MM-dd HH:mm:ss`。`formatStyle` 见 [Sharp 表达式](/sharp-datetime.md)中的日期时间格式化。
* `@FULLDAYNAME(datetime)`或`@FULLWEEKNAME(datetime)` 获取指定日期时间的星期名称，根据语言环境返回值不同，如中文环境是`星期日`，英文是`Sunday`。
* `@FULLMONTHNAME(datetime)` 获取指定日期时间的月份名称，根据语言环境返回值不同，如中文环境是`八月`，英文环境是`Auguest`。
* `@FULLQUARTERNAME(datetime)` 获取指定日期在当年的第几季度和全称，中文环境是`第3季度`，英文环境是`3rd quarter`。
* `@HOUR(datetime)` 获取日期时间中的小时，如`15`。
* `@MICRO(datetime)` 获取日期时间中的微秒，秒后 6 位，如`217000`。
* `@MILLI(datetime)` 获取日期时间中的毫秒，秒后 3 位，如`358`。
* `@MINUTE(datetime)` 获取日期时间中的分钟，如`29`。
* `@MONTH(datetime)`获取月份，如`8`。
* `@MONTHNAME(datetime)` 获取指定日期时间的月份名称，根据语言环境返回值不同，如中文环境是`八月`，英文环境是`Aug`。
* `@NANO(datetime)` 获取指定日期时间的纳秒，秒后 8 位，如`178000000`。
* `@NOW()` 返回当前时间，和全局变量`@NOW`一样。
* `@QUARTER(datetime)` 获取指定日期在当年的第几季度，如八月在第`3`季度。
* `@QUARTERNAME(datetime)` 获取指定日期所在季度的名称，中文环境是`3季`，英文环境是`Q3`。
* `@SECOND(datetime)` 获取日期时间中的秒钟，如`58`。
* `@SEC_TO_TIME(seconds)` 将给定的秒数转成时间，格式为`HH:mm:ss`。如`@SEC_TO_TIME(69635)`结果是`19:20:35`。
* `@TIME_TO_SEC(time)` 将给定的时间转成秒，与`@SEC_TO_TIME()`互为逆操作。如`@TIME_TO_SEC('19:20:35')`结果为`69535`。
* `@TIMESTAMPDIFF(unit, datetime1, datetime2)` 返回两个日期时间之间的差异，`unit`参数可选值为`SECOND|MINUTE|HOUR|DAY|MONTH|YEAR`，单数复数均可。如果`datetime2`早于`datetimem1`，则返回负数。例如`@DATETIMEDIFF(SECOND, '2021-08-29 16:20:30', '2021-08-29 16:25:00')`结果是`4`。
* `@UNIX_TIMESTAMP(datetime)` 将指定日期时间`datetime`转成时间戳（秒），无参数时与`CURRENT_TIMESTAMP()`等效。
* `@WEEKOFYEAR(datetime)`或`@WEEKOFYEAR(datetime, MONDAY)` 获取指定日期所在周是当年的第几周，第二个参数用来指定哪一天是一周的开始，默认`MONDAY`。
* `@YEAR(datetime)` 获取年份，如`2021`。


---
参考链接

* [Sharp 表达式 - 日期时间操作](/sharp-datetime.md)
* [全局函数 FUNCTION](/pql/global-function.md)
* [全局函数 - 字符串操作](/pql/function-text.md)
* [全局函数 - 数字操作](/pql/function-numeric.md)
* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)