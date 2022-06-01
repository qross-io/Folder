<link href='/regex.css' rel='stylesheet' type='text/css'>

# 正则表达式示例 - 边界

边界规则常用于是否匹配整个字符串，或者判断字符串是否以指定规则开始或结束。边界规则常用的有三个：字符串开始`^`、字符串结束`$`和字符组边界`\b`。其中开始`^`和结束`$`在正则表达式中应用非常频繁。

## 完全匹配

一般情况下用于验证用户的输入是否符合指定的规则，比如输入框只允许用户输入 1 位以上的数字。

<span id="Border1" class="regex" copy-text="正则表达式已复制。">^\d+$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border1">复制</button> <button class="small-text-button gray-button" href="/r?text=244691&regex=$(#Border1)%">试试</button>

其中`\d`表示数字，`+`表示重复至少 1 次。由开始符号`^`和结束符号`$`限定，用户只能输入数字，如果输入其他字符则验证会失败。


## 开始于

完全匹配搞定了，判断字符串是否开始于指定规则，只要不限定结束符号就好了。比如下例判断 SQL 语句是不是 SELECT 查询语句。

<span id="Border2" class="regex" copy-text="正则表达式已复制。">^select\s</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border2">复制</button> <button class="small-text-button gray-button" href="/r?text=select * from table1&regex=$(#Border2)%">试试</button>

其中的`\s`表示一个空白字符，空格、换行等都算空白字符。

## 结束于

比如判断一句话是不是问句，即是不是以问号`?`结束。

<span id="Border3" class="regex" copy-text="正则表达式已复制。">\\?$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border3">复制</button> <button class="small-text-button gray-button" href="/r?text=Are you OK?&regex=$(#Border3)%">试试</button>

其中`\?`表示字符问题`?`，因为问号`?`在正则表达式中是特殊字符，所以要用反斜杠`\`转义。其实这里不加斜杠也没什么问题，不过为了能更好理解还是加上的好。

## 字符边界

开始符号`^`和结束符号`$`用于匹配整个字符串的开始和结束，字符边界符号`\b`用于匹配这字符串内部的开始结束，如单词的开始和结束。

匹配以字母`w`开始的单词

<span id="Border4" class="regex" copy-text="正则表达式已复制。">\bw[a-z]+\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border4">复制</button> <button class="small-text-button gray-button" href="/r?text=who why how my when hello&regex=$(#Border4)%">试试</button>

匹配以字母`y`结束的单词

<span id="Border5" class="regex" copy-text="正则表达式已复制。">\b[a-z]+y\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border5">复制</button> <button class="small-text-button gray-button" href="/r?text=who why how my when hello&regex=$(#Border5)%">试试</button>

匹配以字母`w`开始并且以字母`y`结束的单词

<span id="Border6" class="regex" copy-text="正则表达式已复制。">\bw[a-z]+y\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border6">复制</button> <button class="small-text-button gray-button" href="/r?text=who why how my when hello&regex=$(#Border6)%">试试</button>

上例`[a-z]+`表示至少一个英文字母，`+`表示前面的字符规则至少重复一次。

## 非零宽边界

字符边界`\b`全称为零宽字符边界，对应的还有一个`\B`，表示非零宽字符边界。“零宽”表示匹配但不在匹配结果中占用字符，那么“非零宽”就表示匹配但会影响结果。这个规则用得非常少。

<span id="Border7" class="regex" copy-text="正则表达式已复制。">\b[a-f]\B</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border7">复制</button> <button class="small-text-button gray-button" href="/r?text=xy, a3, b2, c4, hello, d2, e0, f7, xyz&regex=$(#Border7)%">试试</button>

实际找不到更好的例子，先凑合看吧。比如有文本“xy, a3, b2, c4, hello, d1, e0, f7, xyz”，我想找到英文字母加数字的匹配，但是只想返回英文字母，数字不需要返回。其中`\B`占了一个字符位，但是不会在结果中返回。


## 按行匹配

字符边界还有一个不常用的场景，开始符号`^`和结束符号`$`用于匹配整个字符串的开始和结束，如果是多行文本的话（有换行符），如果想匹配每一行的开始和结束怎么办？正则表达式中有一个修改符`m`，就为了实现这个功能。


<pre>
<code id="Calendar">
1970,1,1,廿四,,元旦,4,-1
1970,2,12,初七,,,4,-1
1970,2,13,初八,,,5,-1
1970,2,14,初九,,情人节,6,-1
1970,2,18,十三,,,3,-1
1970,2,19,十四,雨水,,4,-1
1970,2,20,十五,,元宵节,5,-1
1970,2,21,十六,,,6,-1
1970,3,5,廿八,,,4,-1
1970,3,6,廿九,惊蛰,,5,-1
</code>
</pre>

假设有上面的文本，我想把每一行的开头的年份都取出来。这里不是为了说明正则表达式怎么写，主要是为了说明`m`修饰符。

正则表达式如下：

<span id="Border8" class="regex" copy-text="正则表达式已复制。">^\d{4}\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Border8">复制</button> <button class="small-text-button gray-button" href="/r?text=$(#Calendar)%&regex=$(#Border8)%&multiline=yes">试试</button>

Javascript 声明为`/^\d{4}\b/m`，Java 的声明为 `Pattern regex = Pattern.compile("^\\d{4}\\b", Pattern.MULTILINE")`，Scala 的声明为`"""(?m)^\d{4}\b""".r`。

关于年份的严谨的正则表达式见 [日期时间示例](/regex/example-datetime.md)。