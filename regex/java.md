# Java 中的正则表达式

直接上代码。

```java
Pattern p = Pattern.compile("<%!(.*?)%>", Pattern.DOTALL);
Matcher m = p.matcher(content);
if (m.find()) {
    setup = m.group(1);
    content = content.replace(m.group(0), "");
}
```

Java 中的正则表达式相关方法有这两个类 Pattern 和 Matcher。Pattern 类用于声明正则表达式并设置修饰符，Matcher 类用于匹配。

## 声明一个正则表达式

只有这一种办法。

```java
Pattern p = Pattern.compile("<%!(.*?)%>", Pattern.DOTALL);
```

Pattern 类有几个常量用于设置修饰符，分别为：

* `Pattern.CASE_INSENSITIVE` 忽略大小写，对应修饰符`i`。
* `Pattern.DOTALL` 不使用这个参数时，符号`.`不匹配回车符和换行符，使用时则`.`匹配所有字符。对应修饰符`s`。
* `Pattern.MULTILINE` 把每行当做一个单独的字符串处理，即`^`和`$`匹配每一行的开始和结束，而不是整个字符串的开始和结束。对应修饰符`m`。

其他还有几个，平时真心用不到，感兴趣的自己百度。

因为修饰符常量都是整数，所有如果在一个正则表达式里使用多个修饰符可以使用`+`号连接。

```java
Pattern p = Pattern.compile("[a-z].{6}", Pattern.CASE_INSENSITIVE + Pattern.DOTALL);
```

另一种修饰符的声明方式，可以将修饰符写在正则表达式最前面，如`(?i)`代表忽略大小写。如上例可以写成：

```java
Pattern p = Pattern.compile("(?is)[a-z].{6}");
```

上例中`(?is)`就是修饰符，是除正则表达式之外的部分。修饰符的这种表示法在使用字符串的相关方法时比较有用，如`matches`或`split`。

## 查找匹配

得到匹配结果要使用到 Matcher 类。

```java
Pattern p = Pattern.compile("[a-z]+", Pattern.CASE_INSENSITIVE);
Matcher m = p.matcher("Hello World!");
if (m.find()) {
    System.out.println(m.group(0));
}
```

其中`m.group(0)`表示整个匹配。除了`group`方法之外，Matcher 类还有一个常用方法`groupCount()`，用于获取每个匹配的分组结果数量。

上例只返回了第一个匹配的结果，如果查找所有匹配的话可以用`while`循环。

```java
while(m.find()) {
    System.out.println(m.group(0));
}
```

可以看出`find()`方法每次执行时会从最后一次匹配的末尾位置可以继续查找，`find()`方法还有一个重载支持参数，表示从指定位置开始匹配，如`find(5)`表示从第 6 个字符开始查找匹配。

## 命名捕获组

Java 和 Javascript 一样也支持匹配结果的捕获组命名，使用`?<name>`，需要写在分组左圆括号`(`的后面。下例`?<month>`、`?<day>`和`?<year>`都是组名。

```java
Pattern p = Pattern.compile("(?<month>\\d{2})/(?<day>\\d{2})/(?<year>\\d{4})");
Matcher m = p.matcher("Today is 04/23/2022.");
while(m.find()) {
    System.out.println(m.group("year") + "-" + m.group("month") + "-" + m.group("day"));
}
//结果是 2022-04-23
```

## 判断是否匹配字符串

一般使用字符串的`matches`方法，但必须是正则表达式必须与字符串整个匹配。

```java
"2022".matches("\\d")  //false
"2022".matches("\\d+") //true
"2022".matches("^\\d+$") //true
```

如果只是判断正则表达式在字符串中是否有匹配，还可以使用 Pattern 类。

```java
Pattern.compile("\\d").matcher("2022").find()  //true
```

## 拆分字符串

将字符串拆分成数组最常用的方法是`split`，特别注意`split`的参数是正则表达式字符串。

```java
"2022|04|23".split("|") //错误，因为 | 是正则表达式特殊字符
"2022|04|23".split("\\|") //正确
```

小技巧：`split`方法默认忽略空值，即当结果字符串的值为空时，将不在结果数组中。

```java
"1,2,,".split(",") //结果是 ["1", "2"] 而不是 ["1", "2", "", ""]
"1,2,,".split(",", -1) //结果是 ["1", "2", "", ""]
```

这个坑儿再分析日志时特别讨厌，解决办法是设置第二个参数，`-1`表示不限制数组的长度，也可以指定一个正整数限制结果数组的长度。