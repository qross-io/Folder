<link href='/regex.css' rel='stylesheet' type='text/css'>

# 正则表达式示例 - 日期时间

相对来说，日期时间的正则表达式比较简单，主要是数字的应用，还有就是理解日期时间规则就好了。为了描述简便，本文所有示例都省略了[边界符号](/regex/example-border.md)。在正则表达式中，1 位数字用`\d`表示，也可以用`[0-9]`代替。

## 年

年份规则比较简单，例如要匹配`19xx`年和`20xx`年。

<span id="DateTime1" class="regex" copy-text="正则表达式已复制。">(19|20)\d{2}</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime1">复制</button> <button class="small-text-button gray-button" href="/r?text=2022&regex=$(#DateTime1)%">试试</button>

其中`19|20`表示年份的前两位，竖线`|`表示“或”关系，用小括号括起来限定竖线的作用域，也就是年份的前两位是一个分组。`{2}`表示前面的字符重复两次，`\d{2}`就表示两位数字，用`\d\d`表示也可以。

## 月

月的规则也不复杂，一年只有 12 个月，两位月份用正则表达式表示如下：

<span id="DateTime2" class="regex" copy-text="正则表达式已复制。">0[1-9]|1[0-2]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime2">复制</button> <button class="small-text-button gray-button" href="/r?text=05&regex=$(#DateTime2)%">试试</button>

竖线`|`左面`0[1-9]`表示`01`到`09`即 1 月到 9 月，右面`1[0-2]`表示`10`到`12`，即 10 月到 12 月，两边是“或”的关系。如果只匹配 1 位月份那么就把开头的`0`去掉就好了。

方括号`[`和`]`中的区间表示只能使用 1 位字符，如`[10-19]`不能表示`10`到`19`，可以用`1[0-9]`或`1\d`表示。

## 日

日的规则稍微复杂些，一个月最多有 31 天，两位的日期用正则表达式表示如下：

<span id="DateTime3" class="regex" copy-text="正则表达式已复制。">0[1-9]|[12][0-9]|3[01]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime3">复制</button> <button class="small-text-button gray-button" href="/r?text=21&regex=$(#DateTime3)%">试试</button>

整个表达式分为三部分，第一部分`0[1-9]`表示`01`到`09`，即 1 到 9 日。第二部分`[12][0-9]`表示`11`到`29`，即 11 日到 29 日，注意`[12]`表示`1`或`2`而不是`12`。第三部分`3[01]`表示`30`和`31`，即 30 日和 31 日。如果匹配 1 或者 2 位日期，把开头和`0`去掉就可以了。

正则表达式不能判断哪个月应该有多少天，比如 4 月有 30 天，也不能判断平年和闰年，需要用程序进行判断。

## 年-月-日

例如要匹配`yyyy-MM-dd`格式的日期，把年、月、日的规则合在一起写可以是：

<span id="DateTime4" class="regex" copy-text="正则表达式已复制。">((?:19|20)\d\d)-(0[1-9]|[12][0-9]|3[01])-(0[1-9]|[12][0-9]|3[01])</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime4">复制</button> <button class="small-text-button gray-button" href="/r?text=2022-05-08&regex=$(#DateTime4)%">试试</button>

表达式使用了 3 组小括号将年月日进行了分组，`19`前面的`?:`表示这个分组是非捕获组，这个分组不会在结果中返回，如果不在意分组，可以去掉。表达式中间的中横线`-`只表示中横线，只有在方括号`[`和`]`中才表示区间。

## 小时

一天 24 小时，即 0 点到 23 点，表达式可以写为：

<span id="DateTime5" class="regex" copy-text="正则表达式已复制。">[01][0-9]|2[0-3]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime5">复制</button> <button class="small-text-button gray-button" href="/r?text=13&regex=$(#DateTime5)%">试试</button>

竖线`|`左面`[01][09]`表示`00`到`19`，即 0 点到晚 7 点。右面`2[0-3]`表示`20`到`23`，即晚上 8 点到 11 点。如果用 12 小时制，那么表达式就是

<span id="DateTime6" class="regex" copy-text="正则表达式已复制。">0[0-9]|1[0-2]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime6">复制</button> <button class="small-text-button gray-button" href="/r?text=05&regex=$(#DateTime6)%">试试</button>

竖线`|`左面`0[0-9]`表示`00`到`09`，即 0 点到 9 点。 右面`1[0-2]`表示`10`到`12`，即 10 点到 12 点。

匹配 1 位小时的话直接把表达式前面的`0`去掉就可以了。

## 分和秒

分钟和秒的表示法是一样的，而且规则相对简单，即`00`到`59`。

<span id="DateTime7" class="regex" copy-text="正则表达式已复制。">[0-5][0-9]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime7">复制</button> <button class="small-text-button gray-button" href="/r?text=52&regex=$(#DateTime7)%">试试</button>

## 时:分:秒

将以上两个规则合起来

<span id="DateTime8" class="regex" copy-text="正则表达式已复制。">([01][0-9]|2[0-3]):[0-5][0-9]:[0-5][0-9]</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime8">复制</button> <button class="small-text-button gray-button" href="/r?text=15:41:03&regex=$(#DateTime8)%">试试</button>

因为小时的规则中包含竖线`|`，为了限制或规则的作用域，在小时规则两端加了小括号。再追加一个 12 小时制的。

<span id="DateTime9" class="regex" copy-text="正则表达式已复制。">(?:0[0-9]|1[0-2]):[0-5][0-9]:[0-5][0-9] [AP]M</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #DateTime9">复制</button> <button class="small-text-button gray-button" href="/r?text=03:41:03 PM&regex=$(#DateTime9)%">试试</button>

小时分组增加了`?:`以减少分组。规则最后的`[AP]M`表示`AM`或`PM`。
