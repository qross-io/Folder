# ChronExp 多功能时间表达式

Cron 表达式是作为调度任务时间计划的行业标准，但是它也有一些小问题，比如对非技术人员不友好、少量场景支持不了等。作者在原生的 Cron 表达式基础上进行了一些扩展，以适用于更多的应用场景。详细使用相关内容参见[Keeper 中的 Cron 表达式](/keeper/cron.md)。

`Chron`比`Cron`多了一个`h`，表示`humanized`，而且本身`chron`也有时间的意思。

## 构造函数

```scala
ChronExp(chronExp: String)
```

`chronExp`参数支持一个或多个标准的 Cron 表达式、1 到 6 位的非标准 Cron 表达式、人性化的周期表达式和地址参数类型的 Cron 表达式。下面几个示例都是系统支持的表达式格式。详见[Keeper 中的 Cron 表达式](/keeper/cron.md)。

* `0 * * * * ? *`
* `0 30 9 * * ? *; 45 10`
* `DAILY 10:00; 12:00`
* `HOUR=2&MINUTE=0`

## 方法

ChronExp 类中的方法很简单。

* `matches(time: DateTime): Boolean` 当前表达式是否能匹配指定的时间。
* `getNextTick(time: DateTime): Option[DateTime]` 按指定时间的查找表达式的下一次匹配。
* `getNextTickOrNone(time: DateTime): String` 查找下一次匹配，如果找到匹配返回日期时间字符串`yyyy-MM-dd HH:mm:ss`，否则返回`N/A`。
* 静态方法`ChronExp.getTicks(chronExp: String, beginTime: String, endTime: String): List[String]` 计算指定表达式在两个日期时间之间的所有匹配，注意参数和返回值都是字符串格式。

Scala 示例

```scala
ChronExp(chronExp).getNextTick(DateTime.now) match {
    case Some(time) => 
    case None =>
}
```

Java示例

```java
String nextTick = new ChronExp(chronExp).getNextTickOrNone(DateTime.now());
```

## 属性

`expression: String` 即构造函数传入的表达式参数，只读。

---
参考链接

* [Keeper 中的 Cron 表达式](/keeper/cron.md)
* [Cron 表达式基础类](/datahub/cron.md)
* [DateTime 日期时间类](/datahub/datetime.md)