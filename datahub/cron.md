# CronExp 基础类

CronExp 类支持`1`到`7`位的 Cron 表达式解析和匹配，注意不支持多个 Cron 表达式和人性化的时间表达式，建议使用 [ChronExp 类](/datahub/chron.md)作为解析的入口。Cron 表达式语法请参见 [Keeper 中的 Cron 表达式](/keeper/cron.md)。

## 构造函数

```scala
CronExp(cronExp: String)
```

参数`cronExp`示例：

* `0 0 2 * *  ? *`
* `0 0` 表示每天`0`点
* `HOUR=12&MINUTE=30` 表示每天`12:30`

## 方法

* `matches(datetime: DateTime): Boolean` 判断表达式是否匹配给定的日期时间
* `matches(datetime: String): Boolean` 判断表达式是否匹配给定的日期时间字符串
* `getNextTick(dateTime: DateTime): Option[DateTime]` 获取给定日期时间的下一次匹配
* `getNextTick(dateTime: String): Option[DateTime]` 获取给定日期时间字符串的下一次匹配
* `getNextTickOrNone(dateTime: DateTime): String` 获取给定日期时间的下一个匹配的时间字符中，如果没有匹配则返回`N/A`
* `getNextTickOrNone(dateTime: String): String` 获取给定日期时间字符串的下一个匹配的时间字符中，如果没有匹配则返回`N/A`
* 静态方法`CronExp.getTicks(cronExp: String, beginTime: String, endTime: String): List[String]`

Scala 示例

```scala
CronExp("0 30 18 * * ? *").getNextTick(DateTime.now) match {
    case Some(time) => 
    case None =>
}
```

Java 示例

```java
String tick = new CronExp("0 30 18 * * ? *").getNextTickOrNone("2020-09-18 12:00:00");
```

## 属性

`expression: String` 即构造函数传入的表达式参数，只读

---
参考链接

* [Keeper 中的 Cron 表达式](/keeper/cron.md)
* [Chron 时间表达式](/datahub/chron.md)
* [DateTime 日期时间类](/datahub/datetime.md)