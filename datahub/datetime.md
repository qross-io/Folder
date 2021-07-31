# DateTime 日期时间类

封装这个类的原因是因为Java原生的日期时间类太难用了，后来虽然有了`LocalDateTime`类，也是各种不习惯。平时开发中时常要用到日期时间，特别是数据开发时更会频繁用到。DateTime 类封装了`LocalDateTime`类并提供了大量常用的方法进行相关的操作。


## 构造函数

* `DateTime()`
* `DateTime(dateTime: Any)`
* `DateTime(dateTime: Any , formatStyle: String)`
* `DateTime(dateTime: Any, mode: Int)`
* `DateTime(dateTime: Any, formatStyle: String, mode: Int)`

相关参数解释如下：

* `dateTime` 任何格式的日期时间值，如字符串`2020-09-12 12:00:00`或纪元秒等，系统会自动判断类型并尝试转化为正确的日期时间。
* `formatStyle` 当系统不能将`dateTime`参数正常识别或转换时，可辅助`formatStyle`参数，如`new DateTime("2019年9月20日", "yyyy年M月d日")`
* `mode` 日期时间类型参数，可选`DateTime.DATE`、`DateTime.TIME`、`DateTime.FULL`、`DateTime.TIMESTAMP`，分别代表仅日期、仅时间、完整格式和时间戳。

## 静态方法

静态方法主要用于辅助创建时间。

* `now: DateTime` 获得当前日期时间。
* `today: DateTime` 获得今天的日期，时间为`00:00:00`。
* `from(date: DateTime): DateTime` 从另一个日期时间创建，一般用于复制。
* `ofTimestamp(epochSecond: Long): DateTime` 从纪元秒新建日期时间。
* `of(year: Int, month: Int, dayOfMonth: Int, hourOfDay: Int, minute: Int, second: Int): DateTime` 指定日期时间的各部分新建。
* `getDaysSpan(beginTime: String, endTime: String): Long` 获得两个日期时间之间的天数差。
* `getDaysSpan(beginTime: DateTime, endTime: DateTime): Long` 获得两个日期时间之间的天数差。
* `getSecondsSpan(beginTime: String, endTime: String): Long` 获得两个日期时间之间的秒数差。
* `getSecondsSpan(beginTime: DateTime, endTime: DateTime): Long` 获得两个日期时间之间的秒数差。

## 实例方法

实例方法是DateTime类的核心，分组说明如下：

### 获取日期时间中的某个单位

* `get(field: ChronoField): Int` 根据`ChronoField`得到相应的日期时间部分的值，不常用。
* `getYear: Int` 得到年份。
* `getMonth: Int` 得到月份。
* `getDayOfMonth: Int` 得到日期，即当月几号。
* `getDayOfWeek: Int` 得到星期数字，周一到周日分别对应`1`到`7`。
* `getWeekName: String` 得到星期的名称，中英文环境下返回值不同。
* `getHour: Int` 得到`24`小时制的小时。
* `getMinute: Int` 得到分钟。
* `getSecond: Int` 得到秒。
* `getMilli: Int` 得到毫秒数。
* `getMicro: Int` 得到微秒数。
* `getNano: Int` 得到纳秒数。

下面是以上方法的简写：

* `year`
* `month`
* `dayOfMonth`
* `dayOfweek`
* `weekName`
* `hour`
* `minute`
* `second`
* `milli`
* `micro`
* `nano`

### 设置时间的某个单位的值

* `set(filed: ChronoField, value: Int): DateTime` 按`ChronoField`设置某个单位的值，不常用。
* `setYear(year: Int): DateTime` 设置年份。
* `setMonth(month: Int): DateTime` 设置月份。
* `setDayOfMonth(day: Int): DateTime` 设置日期。
* `setDayOfWeek(week: String): DateTime` 按英文星期名称设置星期，最少两位，如`setDayOfWeek('WED')`。名称不区分大小写。
* `setDayOfWeek(week: Int): DateTime` 按数字设置星期，星期一到星期日分别对应`1`到`7`。
* `setHour(hour: Int): DateTime` 设置小时。
* `setMinute(minute: Int): DateTime` 设置分钟。
* `setSecond(second: Int): DateTime` 设置秒钟。
* `setMilli(milli: Int): DateTime` 设置毫秒。
* `setMicro(micro: Int): DateTime` 设置微秒。
* `setNano(nano: Int): DateTime` 设置纳秒。
* `setBeginningOfMonth(): DateTime` 设置为月初`1`号`00:00:00`。
* `setZeroOfDay(): DateTime` 设置为当天`00:00:00`。

### 日期时间的增减操作

* `plus(unit: ChronoUnit, amount: Int): DateTime` 在当前日期时间基础上增加指定数量的时间单位`ChronoUnit`，不常用。
* `plus(unit: String, amount: Int): DateTime` 在当前日期时间基础上增加指定数量的时间单位`unit`，不常用。`unit`支持`YEAR`、`MONTH`、`DAY`、`HOUR`、`MINUTE`、`SECOND`、`MILLI`、`MICRO`、`NANO`，不区分大小写。
* `plusYears(amount: Long): DateTime` 增加年。
* `plusMonths(amount: Long): DateTime` 增加月。
* `plusDays(amount: Long): DateTime` 增加天。
* `plusHours(amount: Long): DateTime` 增加小时。
* `plusMinutes(amount: Long): DateTime` 增加分钟。
* `plusSeconds(amount: Long): DateTime` 增加秒。
* `plusMillis(amount: Long): DateTime` 增加毫秒。
* `plusMicros(amount: Long): DateTime` 增加微秒。
* `plusNanos(amount: Long): DateTime` 增加纳秒。
* `minus(unit: ChronoUnit, amount: Int): DateTime` 在当前日期时间基础上减少指定数量的时间单位`ChronoUnit`，不常用。
* `minus(unit: String, amount: Int): DateTime` 在当前日期时间基础上减少指定数量的时间单位`unit`，不常用。`unit`支持`YEAR`、`MONTH`、`DAY`、`HOUR`、`MINUTE`、`SECOND`、`MILLI`、`MICRO`、`NANO`，不区分大小写。
* `minusYears(amount: Long): DateTime` 减少年。
* `minusMonths(amount: Long): DateTime` 减少月。
* `minusDays(amount: Long): DateTime` 减少天。
* `minusHours(amount: Long): DateTime` 减少小时。
* `minusMinutes(amount: Long): DateTime` 减少分钟。
* `minusSeconds(amount: Long): DateTime` 减少秒。
* `minusMillis(amount: Long): DateTime` 减少毫秒。
* `minusMicros(amount: Long): DateTime` 减少微秒。
* `minusNanos(amount: Long): DateTime` 减少纳秒。

日期时间的增减操作支持负数参数，如`plusDays(-1)`，等同于`minusDays(1)`。`plus`和`minus`操作跟`set`操作都返回日期时间对象本身，所以可以进行链式操作，如`DateTime.today.plusMonths(1).setDayOfMonth(1).minusDays(-1)`得到本月最后一天。

### 转换和格式化操作

* `getString(formatStyle: String): String`或`format(formatStyle: String): String` 按指定的格式格式化日期时间，返回字符串。
* `getNumber(formatStyle: String): Long` 按指定的格式格式化日期时间，返回整数。
* `toEpochSecond: Long` 得到`10`位的纪元秒。
* `toEpochMill: Long` 得到`13`位的纪元毫秒。
* `toDate: java.util.Date` 得到Java的原生日期时间类。
* `toString: String` 根据`mode`不同返回不同格式的字符串，默认`yyyy-MM-dd HH:mm:Ss`。
* `toDateString: String` 返回格式为`yyyy-MM-dd`。
* `toTimeString: String` 返回格式为`HH:mm:ss`。
* `toFullString: String` 返回格式为`yyyy-MM-dd HH:mm:ss`。

### `formatStyle`参数中的时间单位

* **`y`** 表示年，2位返回`19`，4位返回`2019`
* **`M`** 表示月，1位返回`4`，2位返回`04`，3位返回`Apr`，4位返回 `April`。中文环境下返回中文，如`四月`。
* **`d`** 表示天，1位和2位都返回`15`，1位时小于`10`才返回1位数字
* **`H`** 表示24小时制小时，1位和2位都返回`13`，1位时小于`10`返回1位数字
* **`h`** 表示12小时制小时，1位返回`1`，2位返回`01`
* **`m`** 表示分钟，1位和2位都返回`27`，1位时分钟小于`10`时返回1位数字。
* **`s`** 表示秒，1位返回`5`，两位返回`05`
* **`S`** 表示毫秒，一般为3位
* **`e`** 表示星期，1位返回`1`，2位返回`01`，3位返回`Mon`，4位返回`Monday`。中文环境下返回中文，如`星期一`。
* **`a`** 表示上下午，返回`AM`或`PM`。中文环境下返回`上午`或`下午`。

### 日期时间的比较操作

* `equals(otherDateTime: DateTime): Boolean` 是否相等。
* `before(otherDateTime: DateTime): Boolean` 是否早于另一个时间。
* `beforeOrEquals(otherDateTime: DateTime): Boolean` 是否早于或等于另一个时间。
* `after(otherDateTime: DateTime): Boolean` 是否晚于另一个时间。
* `afterOrEquals(otherDateTime: DateTime): Boolean` 是否晚于或等于另一个时间。
* `later(otherDateTime: DateTime): Long` 当前日期时间晚于另一个日期时间多少毫秒，可能为负数。
* `earlier(otherDateTime: DateTime): Long` 当前日期时间早于另一个日期时间多少毫秒，可能为负数。
* `span(otherDateTime: DateTime): Long` 当前日期时间与另一个日期时间相差多少毫秒，`0`或正数。

### 其他操作

* `matches(chronExp: String): Boolean` 是否匹配指定的Chron时间表达式。
* `copy(): DateTime` 复制当前日期时间
* `express(expression: String): DateTime` 通过表达式快速设置日期时间。
    例如：`DateTime.now.express('MONTH+1#DAY=1#DAY-1#HOUR=0#MINUTE=0#SECOND=0#NANO=0')`
    或`DateTime.now.express('DAY=L#HOUR=0#MINUTE=0#SECOND=0#NANO=0')`，等同于`DateTime.now.plusMonths(1).setDayOfMonth(1).minusDays(1).setHour(0).setMinute(0).setSecond(0).setNano(0)`。

    + `expression`由一个或多个时间表达式构成，时间表达式之间用井号`#`隔开。
    + 每个时间表达式由“时间单位”+“操作符”+“值”构成。
    + 支持的时间单位有：`YEAR`,`MONTH`,`DAY`,`WEEK`,`MINUTE`,`SECOND`,`MILLI`,`MICRO`,`NANO`。时间单位不区分大小写。
    + 支持的操作符有：`=`表示设置，`+`表示增加，`-`表示减去。
    + 值部分除了支持各时间单位允许的整数值。
    + 设置`DAY`时支持`L`，表示最后一天。
    + 设置`WEEK`时支持三位的星期名称：Mon,Tue, Wed, Thu, Fri, Sat, Sun
* `getTickValue()` 得到精确到分钟的当前时间，秒位为`0`，如`2021-07-28 19:28:00`。
* `getTockValue()` 得到精确到小时的当前时间，分钟和秒位为`0`，如`2021-07-28 19:00:00`。

## 属性

* `localDateTime` 得到Java的 **LocalDateTime** 日期时间类

---
参考链接

* [ChronExp 多样化时间表达式](/datahub/chron.md)
* [CronExp Cron表达式基础类](/datahub/cron.md)
* [PQL中日期时间相关的Sharp表达式](/pql/sharp-datetime.md)