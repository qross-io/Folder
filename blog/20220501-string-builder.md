# 大字符串拼接问题优化

这是一个在 PQL 解析过程中遇到一个大字符串处理的效率问题，这个大字符串文本保存在文件中，大约 1.5M。原始程序是这样的：

```scala
blocks.foreach(closed => {
        val before = sentence.take(closed.start)
        val after = sentence.substring(closed.end)
        //val after = sentence.takeAfter(closed.end - 1)
        val replacement = {
            closed.name match {
                //...
                    PQL.chars += sentence.substring(closed.start, closed.end)
                //...
            }
        }

        sentence = before + replacement + after
    })
```

其中：

* `blocks`是一个先进后出的栈 ArrayStack，里面存着大字符串文本中解析出来的子字符串信息，主要是子字符串在大字符串文本中的开始位置和结束位置
* 程序目的是根据这些子字符串的开始位置和结束位置将子字符串截取出来，再替换成其他的字符串`replacement`。
* 这个程序从字符串后向前取，先根据开始位置取大字符串前面的内容`before`，再根据结束位置取大字符串后面的内容`after`，最后再和要替换的内容一起拼接成一个新字符串。
* 其中的非关键代码已省略掉。

在整个字符串长度相对短时，这个程序的效率没多大问题。但当整个字符串特别大且`blocks`中要截取子字符串特别多时，这是一个不断拼接大字符串的操作，且每次都给变量`sentence`重新赋值。整个过程计时 227283 毫秒，接近 4 分钟！

这是我之前写的代码，脸红啊！优化后的代码如下：

```scala
val sb = new mutable.StringBuilder()
var cursor = 0
for (i <- blocks.indices.reverse) {
    val closed = blocks(i)
    sb.append(sentence.substring(cursor, closed.start))
    sb.append(
        closed.name match {
            //...
                PQL.chars += sentence.substring(closed.start, closed.end)
            //...
        }
    )
    cursor = closed.end
}
sb.append(sentence.substring(cursor))
sentence = sb.toString()
```

解释：

* 由从后向前遍历改为从前向后遍历`reverse`。
* 过程中没有字符串拼接和字符串赋值操作。
* 使用 StringBuilder 对象拼接截取下来的字符串。
* 每次遍历截取次数也少了一次。
* 最后才将 StringBuilder 的值赋给结果变量`sentence`。

优化后耗时为 87 毫秒！效率提升千倍不止。

（本文完）

