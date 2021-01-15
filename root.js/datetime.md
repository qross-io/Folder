# DateTime 类

**DateTime** 不是组件，是对Javascript原生日期时间类 **Date** 的扩展，因为`Date`类实在太难用了。

## 构造函数

```javascript
new DateTime(value);
```

参数`value`接受的数据类型如下：

* `null`即不传值，默认当前时间
* `Date`原生Date类型
* 时间字符串，接受的格式比较多，分别列表如下：
    + `yyyy` 只有年，其他单位默认为`01-01 00:00:00`
    + `yyyyMM` 只有年和月，其他单位默认为`01 00:00:00`
    + `yyyyMMdd` 只有年月日，时间单位默认为`00:00:00`
    + `yyyyMMddHH` 缺少分钟和秒，默认为`00:00`
    + `yyyyMMddHHmm` 缺少秒，默认为`00`
    + `yyyyMMddHHmmss`
    + `yyyy-MM` 有年和月，其他单位默认为`01 00:00:00`
    + `yyyy-MM-dd` 有年月日，其他单位默认为`00:00:00`
    + `yyyy-MM-dd HH` 缺少分钟和秒，默认为`00:00`
    + `yyyy-MM-dd HH:mm` 缺少秒，默认为`00`
    + `yyyy-MM-dd HH:mm:ss`
    + `yyyy/MM`
    + `yyyy/MM/dd`
    + `yyyy/MM/dd HH`
    + `yyyy/MM/dd HH:mm`
    + `yyyy/MM/dd HH:mm:ss`

上面使用了时间格式符号，除了上面涉及到的外，还有毫秒，用大写`S`表示，区别于小写`s`的秒。

## 静态方法

* `of(year, month, dayOfMonth, hour, minute, second, milli)` 使用数字构造时间，返回一个DateTime对象。如`DateTime.of(2020, 9, 17)`
* `now()` 获取当前日期时间，返回DateTime对象。如`DateTime.now()`
* `today()` 获取当前日期，返回DateTime对象。如`DateTime.today `()`

* `getCalendar(year, month, firstDay = 'MON')` 获取指定年月的月历，结果是一个`6`行`7`列的二维数组，可通过`fristDay`指定每周的第一天是周几

## 实例方法

* `get(format = 'yyyy-MM-dd HH:mm:ss')` 按指定格式返回日期时间字符串，如`DateTime.now().get('yyyy年H月d日')`，返回`2020年9月18日`
* `getYear()` 返回年份，值为整数
* `getMonth()` 返回整数月份
* `getDayOfMonth()` 返回整数日期
* `getWeek()` 返回周，值为整数。`1`到`6`分别对应周一到周六，周日为`0`
* `getHour()` 返回整数小时
* `getHourString()` 返回两位字符串格式的小时，小于`10`自动补零，如`09`、`18`等
* `getMinute()` 返回整数分钟
* `getMinuteString()` 返回两位字符串格式的分钟，小于`10`自动补零
* `getSecond()` 返回整数秒
* `getSecondString()` 返回两位字符串格式的秒，小于`10`自动补零
* `getMilli()` 获取整数毫秒
* `getWeekOfYear()` 获取当前日期是当年的第几周
* `setYear(year)` 设置年份
* `setMonth(month = 1)` 设置月份
* `setDayOfMonth(day = 1)` 设置日期。可传递`L`或`31`设置到当月最后一天
* `setHour(hour = 0)` 设置小时
* `setMinute(minute = 0)` 设置分钟
* `setSecond(second = 0)` 设置秒
* `setMilli(milli = 0)` 设置毫秒

* `before(other)` 判断当前时间是否早于另一个时间
* `beforeOrEquals(other)` 判断当前时间是否早于或等于另一个时间
* `after(other)`  判断当前时间是否晚于另一个时间
* `afterOrEquals(other)` 判断当前时间是否晚于或等于另一个时间
* `between(begin, end)` 判断当前时间是否在两个时间之间

* `monthsSpan(other)` 计算当前日期时间和另一个日期时间的月数差，当前日期大于比较日期为正，返回整数
* `daysSpan(other)` 计算当前日期时间和另一个日期时间的天数差，返回整数，当前日期大于比较日期为正
* `hoursSpan(other)` 计算当前日期时间和另一个日期时间的小时差，当前日期时间大于比较日期时间为正，返回整数
* `minutesSpan(other)` 计算当前日期时间和另一个日期时间的分钟差，当前日期时间大于比较日期时间为正，返回整数
* `secondsSpan(other)` 计算当前日期时间和另一个日期时间的秒数差，当前日期时间大于比较日期时间为正，返回整数
* `later(other)` 计算当前时间晚于另一个时间多少秒，如果当前时间早于另一个时间，则返回负数
* `earlier(other)` 计算当前时间早于另一个时间多少秒，如果当前时间晚于另一个时间，则返回负数
* `span(other)` 计算两个时间的秒数差的绝对值

* `plusYears(years)` 在当前日期时间基础上加上`years`年，`years`可以为负数
* `plusMonths(months)` 在当前日期时间基础上加上`months`月，`months`可以为负数
* `plusDays(days)` 在当前日期时间基础上加上`days`天，`days`可以为负数
* `plusHours(hours)` 在当前日期时间基础上加上`hours`小时，`hours`可以为负数
* `plusMinutes(minutes)` 在当前日期时间基础上加上`minutes`分钟，`minutes`可以为负数
* `plusSeconds(seconds)` 在当前日期时间基础上加上`seconds`秒，`seconds`可以为负数
* `plusMillis(millis)` 在当前日期时间基础上加上`millis`毫秒，`millis`可以为负数

* `toString()` 返回格式为`yyyy-MM-dd HH:mm:ss`的日期时间
* `toDateString()` 返回格式为`yyyy-MM-dd`的日期
* `toTimeString()` 返回格式为`HH:mm:ss`的时间

* `getGanZhiYear()` 获取当前年的天干地支中文名称
* `getZodiac()` 获取当前年的生肖的中文名称
* `getLunarDay()` 获取农历，如果是初一显示农历月份，需要日历`Calendar`组件支持
* `getFestival()` 获取节日名称，需要日历组件支持。非节日返回`N/A`
* `getWorkday()` 获取工作日信息，`0`表示正常休息日，`1`表示正常工作日，`2`表示特殊休息日，如法定节假日，`3`表示特殊工作日，如法定节节假日调班。需要日历组件支持

* `copy()` 复制差生成一个新的日期时间实例。

特别说明，`set`和`plus`操作返回对象本身，所以可以对多个方法进行链式编写，如`DateTime.now().setHour(0).setMinute(0).plusDays(1)`。再有实例对象变更时不需要再重新赋值，如下例：
```javascript
let today = DateTime.today();
today.plusMonths(1).setDayOfMonth(1).plusDays(-1);
```
而不是
```
today = today.plusMonths(1).setDayOfMonth(1).plusDays(-1);
```
这一点与基本类型不同，在使用中尤其注意。如果不希望在操作过程中更改原来的对象，可用`copy()`方法生成一个新实例。

## 字符串扩展

* `toDateTime()` 将一个字符串时间转成日期时间类型，如`'2020-09-18'.toDateTime()`，等同于`new DateTime('2020-09-18')`
* `toDateInt(format = 'yyyy-MM-dd')` 将一个字符串格式的日期时间转成整数，需要指定日期时间的格式
* `isDateTime()` 是不是标准的日期时间字符串，如`'2020-09-18 15:21:00'.isDateTime()`返回`true`
* `isDate()` 是不是标准的日期字符串，如`'2020-09-18'.isDate()` 返回`true`
* `isTime()` 是不是标准的时间字符串，如`'12:55'.isTime()` 返回`true`

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Calendar 日历组件](/root.js/calendar.md)
* [Clock 文字时钟组件](/root.js/clock.md)