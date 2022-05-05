# Scala 中的正则表达式

Scala 可以用 [Java 的方法](/regex/java.md)操作，也有一些自己的方法，有时用起来更简单一些。

```scala
"\\d+".r.findFirstIn("2022-04-23") match {
    case Some(y) => println(y) //找到匹配，结果是 2022
    case None => //找不到匹配走这个逻辑
}
```

## 声明正则表达式

上例中的`"\\d".r`即 Scala 语言中的正则表达式声明，也可以使用富字符串省略一个反斜杠转义。

```scala
"""\d+""".r
```

如果要使用修饰符，则需要写在正则表达式的前面，如`(?i)`表示忽略大小写，常用的有三个：

* `i` 忽略大小写。
* `s` 点符号`.`也匹配回车和换行符，默认不匹配。
* `m` 将一个多行字符串按照换行符看作多行进行处理，即开始符`^`和结束符`$`匹配每一行的开头和结尾，而不再是整个字符串的开始和结尾。

一个完整的正则表达式对象声明如下：

```scala
val r: Regex = """(?i)[a-z]+""".r
```

## 常用方法

常用方法有 6 个。第一个是`findFirstIn`，即在字符串中找到第一个匹配，即本文第一个示例。也可以用这个方法判断是否可以找到匹配。

```scala
"""\d+""".r.findFirstIn("2022-04-23").nonEmpty  //true
```

第二个方法是`findAllIn`，用来查找所有匹配字符串，返回一个结果集迭代器，可以用各种集合方法进行处理。

```scala
"""\d+""".r.findAllIn("2022-04-23").foreach(println)
```

第三个方法是`findFirstMatchIn` 用于返回第一个匹配，返回值类型是`Regex.Match`，当使用分组要用到。

```scala
"""(\d{4})-(\d{2})-(\d{2})""".r.findFirstMatchIn("2022-04-23") match {
    case Some(m) => //找到匹配
            println(m.group(0)) //2022-04-23
            println(m.group(1)) //2022
            println(m.group(2)) //04
            println(m.group(3)) //23
    case None => //找不到匹配走这个逻辑
}
```

第四个方法是`findAllMatchIn` 即返回所有匹配的集合，可以用数组方法进行迭代。

第五个方法是`findPrefixIn` 用来查找一个前缀匹配，即默认在正则表达式前面加符号`^`，其他同`findFirstIn`。

第六个方法是`findPrefixMatchIn` 和上一个方法类似，不过是返回匹配对象`Regex.Match`。

## 匹配结果对象

上一节所有带`Match`的方法都返回匹配结果对象，对应的类是`Regex.Match`。这里简单介绍一下这个类的方法和属性。

* `group(i)` 最常用，按索引获得匹配分组的值，和其他语言一样，其中`0`表示整个匹配，`1`表示第一个分组，依次类推。
* `group(name)` 在正则表达式中可以对分组进行命名，按分组名返回匹配结果，见下一节。
* `groupCount` 分组数量，不包括分组`0`。
* `start` 当前匹配在原始字符串的索引开始位置
* `end` 当前匹配在原始字符串的索引结束位置
* `after` 当前匹配后面的所有字符，如果当前匹配后面没有字符则返回空字符串
* `start(i)` 指定某一个分组`i`在原始字符串的索引开始位置
* `end(i)` 指定某一个分组`i`在原始字符串的索引结束位置
* `after(i)` 指定某一个分组`i`后面的所有字符，如果指定分组`i`后面没有字符则返回空字符串
* `source` 原始字符串

如果某个分组不能匹配到结果，则结果为`null`，分组数量`groupCount`上也会减少。

## 命名捕获组

Scala 和 Java 和 Javascript 一样支持对捕获分组进行命名，语法相同，也一样是`?<name>`，写在分组内左括号的后面。

```scala
"(?<month>\\d{2})/(?<day>\\d{2})/(?<year>\\d{4})".r.findFirstMatchIn("04/23/2022") match {
    case Some(m) =>
        println(m.group("year"))
        println(m.group("month"))
        println(m.group("day"))
    case None => println("none")
}
```

## 字符串相关方法

与 Java 一样，字符串的`matches`和`split`方法参数也是正则表达式，可以参见 [Java 相关操作](/regex/java.md)中的说明。