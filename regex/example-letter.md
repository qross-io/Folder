
# 正则表达式示例 - 字母

字母包括大写 A 到 Z 和小写 a 到 z，在实际应用中非常多。

## 单词

在正则表达式中，不像数字有一个`\d`来表示，要表示字母，只能使用`[A-Z]`或`[a-z]`。先来个最简单的，匹配单词。

<span id="Letter1" class="regex" copy-text="正则表达式已复制。">[a-z]+</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter1">复制</button> <button class="small-text-button gray-button" href="/r?text=hello world%&regex=$(#Letter1)%">试试</button>

其中`+`表示匹配一次或多次，即单词最少一个字母，最多不限制。任何程序语言或者工具都有忽略大小写的修饰符选项，也可以在正则表达式中控制，如可以写成`[A-Za-z]`或者将修饰符写在正则表达式的最前面`(?i)[a-z]+`（有的语言不支持这样写，如 Javascript）。

上面的正则表达式还有 Bug，比如原始文本里有`hao123`这个字符串，上面的正则表达式会把`hao`也匹配出来，下一步我们把这种情况去掉。

<span id="Letter2" class="regex" copy-text="正则表达式已复制。">\b[a-z]+\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter2">复制</button> <button class="small-text-button gray-button" href="/r?text=hello hao123%&regex=$(#Letter2)%">试试</button>

其中的`\b`表示边界，即边界内只允许指定的字符，除此之外的字符都不允许。上例只能匹配到`hello`，但是不能匹配以`hao`。

再提更多要求，比如匹配首字母大写的单词。这时就不能开启忽略大小写修饰符了，正则表达式这样写。

<span id="Letter3" class="regex" copy-text="正则表达式已复制。">\b[A-Z][a-z]*\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter3">复制</button> <button class="small-text-button gray-button" href="/r?text=Hello world%&regex=$(#Letter3)%&ignore=yes">试试</button>

其中`[A-Z]`表示首字母必须大写，`*`表示匹配 0 次或多次，即允许 1 个大写字母这样的单词，如我`I`、一个`A`等。

## 变量

一般程序里命名变量允许的字符只有字母、数字和下划线`_`，而且不允许以数字开头。我们来搞定这个。

<span id="Letter4" class="regex" copy-text="正则表达式已复制。">\w+</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter4">复制</button> <button class="small-text-button gray-button" href="/r?text=Hello_world 123%&regex=$(#Letter4)%">试试</button>

没看错，只有一个`\w`，重复 1 次或多次。在正则表达式中，`\w`就表示字母、数字和下下划线`_`。但是，简单是简单，这个表达式不对。数字开头或者全是数字也会被匹配出来，而变量的要求不能以数字开头。

<span id="Letter5" class="regex" copy-text="正则表达式已复制。">[A-Za-z_]\w*</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter5">复制</button> <button class="small-text-button gray-button" href="/r?text=_hello world _%&regex=$(#Letter5)%">试试</button>

其中`[A-Za-z_]`表示匹配开头必须是字母或下划线，后面的`\w*`有示 0 位或多位变量允许的字符。等等，好像还有个 Bug，一个下划线也会被匹配到。虽然有的语言允许一个下划线作为变量名，如 Java、Javascript，但是谁也不会这么做。仅且不允许吧，继续修改一下。

<span id="Letter6" class="regex" copy-text="正则表达式已复制。">[A-Za-z]|[A-Za-z_]\w+</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter6">复制</button> <button class="small-text-button gray-button" href="/r?text=_hello world _%&regex=$(#Letter6)%">试试</button>

这样就好了，一位字母变量和两位以上的以字母或下划线开头的变量是被允许的。其实，这个表达式还不完美，变量名中除了下划线外，美元符号`$`也是被允许的。再加上边界控制，最终的表达式是下面这样：

<span id="Letter7" class="regex" copy-text="正则表达式已复制。">\b[A-Za-z$]|[A-Za-z_$]\w+</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Letter7">复制</button> <button class="small-text-button gray-button" href="/r?text=_hello world _%&regex=$(#Letter7)%">试试</button>

这时放在方括号中的`$`就不再表示匹配结尾了，只表示一个常量字符`$`，但是在方括号外面时，必须加上转义符，即写成`\$`。

（本文完）