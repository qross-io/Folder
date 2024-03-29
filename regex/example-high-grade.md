<link href='/regex.css' rel='stylesheet' type='text/css'>

# 正则表达式高级应用示例

这里汇总整理老吴开发过程中遇到的一些比较复杂的正则表达式应用示例，适合有一定经验的开发者借鉴。

## 断言应用

这是一个 root.js 标签库 [Express 字符串](/root.js/express.md)中对 URL 地址进行编码的一个功能。假如原始文本是这样的：

```string
/api/search?keyword=我是关键词%&exp=^\d+$%
```

主要意图是将字符串中的`我是关键词`和`^\d+$`进行编码，让数据在前后端传递过程中避免出错，百分号`%`就是告诉程序要进行编码的意思。所以要把字符串需要的部分取出来。

正则表达式编写要求和思路：

* 取出要编码的常量值和常量值后面的百分号`%`，然后才能替换成编码后的值。
* 前面必须是等号`=`，因为只需要对地址中的参数值进行编码。
* 必须以百分号`%`结束，且百分号后面必须是与号`&`或井号`#`或字符串的结尾，这是因为地址参数之间是通过与号`&`分隔的，井`#`号是页内定位符，在整个地址的后面。
* 常量是分组结果 1，方便进行替换操作。

好了，下面一步一步来。

* 第一步，先把基本的表达式写出来。

<span class="regex">(.*?)%</span>

因为不知道常量值是什么，所以使用圆点`.`表示除换行符以外的所有字符。星号`*`表示匹配 0 次或多次，因为参数值可以为空，所以不用加号`+`。问号`?`表示非贪婪模式，即匹配尽可能少的字符。小括号用来捕获分组结果，即常量值。

* 第二步，前面必须是等号`=`。得用断言了，因为你不能把等号也在结果中返回来，虽然可以但是不方便后面程序处理。

<span class="regex">(?<==)(.*?)%</span>

前面加的那部分`(?<==)`叫零宽正向后行断言，“零宽”表示匹配指定的字符但匹配上的字符在结果中不返回，“正向”表示肯定，“后行”表示匹配表达式的左面（字符串是从左向右读，所以向左是后行）。肯定难明白，即使是中国字也不好明白。再分解一些`(?`和`)`表示零宽，左尖括号`<`表示后行，第一等号`=`表示正向即肯定。第二个等号是我们要求匹配结果前面必须是等号`=`的等号，如果实在不好阅读可以写成`[=]`或`(=)`，全写出来就是`(?<=[=])`或`(?<=(=))`。

* 第三步，百分号后面必须是与号`&`或井号`#`或字符串结束。

<span class="regex">(?<=(=)>)(.*?)%(?=(&|#|$))</span>

后面加的那部分`(?=(&|#|$))`叫零宽正向先行断言，“先行”表示匹配表达式的右面，其他同上。即要求匹配后面必须是指定的字符。这里`(&|#|$)`表示三选一，`$`表示字符串的结束，不能用`[&#$]`，因为`$`在方括号里就是个常量了，不能表示字符串结尾了。

* 第四步，减少匹配结果的分组。上面有三个小括号构成的分组，分别是`(=)`、`(.*?)`、`(&|#|$)`，两个断言不会在匹配结果中返回。这三个分组只有第二个我们需要，所以要让前两个分组变成“非捕获组”，即在左小括号后面加上`?:`就行了。

<span id="Text1" hidden>/api/search?keyword=我是关键词%&ex=^\d+$%</span>

<span id="Final1" copy-text="正则表达式已复制。" class="regex">(?<=(?:=))(.*?)%(?=(?:&|#|$))</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Final1"> 复制</button> <button class="small-text-button gray-button" href="/r?text=$(#Text1)%&regex=$(#Final1)%">试试</button>

## 多个分组和空白字符

有这么一个需求，判断给定字符串是不是 JSON 对象格式的字符串，判断的目的是和 Javascript 语句区分开来。开始分析，一个完整格式的 JSON 对象字符串由大括号`{`和`}`包围，其中有多个项，每个项由键和值构成，键和值之间使用冒号`:`分隔，键名使用双引号`"`引起来，值可以有不同类型，多个项之间使用逗号`,`分隔开。

一步一步来，先实现开始和结束由大括号包围。

<span class="regex">^\s\*\{[^{}]\*?\}\s\*$</span>

挨个解释。

* `^`和`$`表示匹配字符串的开始的结束。
* `\s*`表示匹配 0 个或多个空白字符，因为字符串的开始和结尾可能包含空格，需不需视情况而定。
* `\{`和`\}`有示匹配字符`{`和`}`，因为大括号在正则表达式的保留字符，表示重复次数，所以要加转义符号`\`。
* `[^{}]*?`中的`[^{}]`表示匹配除左右大括号以外的所有字符，保留符号在方括号`[`和`]`中时一般可以不加转义符号，当然加上也没有问题。这里的`^`不再表示字符串的开始，表示`^`后面的字符不再匹配期望之内。星号`*`表示匹配 0 次或多次，星号`*`后面的问题`?`表示非贪婪匹配模式，匹配尽可能少的字符，否则匹配尽可能多的字符，这里其实可以省略。

下面增加项的键名规则，由双引号引用，即

<span class="regex">"[^"]\*"\s\*:</span>

* `"[^"]*"`匹配整个键名，这里用星号`*`而不用加号`+`是因为键名可能为空。
* `\s*`表示键名和冒号`:`之间可能有空白字符，比如空格。

两个正则表达式合在一起，就可以表示一个不为空的对象字符串。

<span class="regex">^\s*\{\s*"[^"]\*"\s\*:[^{}]*?\}\s\*$</span>

这个表达式没那么严谨，因为不会判断这个对象有多少项，每一项是否都符合规则，可以说只匹配了开头。目的只要和 Javascript 语句区分开来就好。

对象字符串有可能为空，现在要加上这个规则。

<span id="Text2" hidden>{ "key1": 1, "key2": "value2" }</span>

<span id="Final2" copy-text="正则表达式已复制。" class="regex">^\s\*\{\s\*(?:"[^"]*"\s\*:[^{}]\*?|)\}\s\*$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Final2"> 复制</button> <button class="small-text-button gray-button" href="/r?text=$(#Text2)%&regex=$(#Final2)%">试试</button>

区别就是使用了分组，并且有一个空可选项`|)`，前面的`(?:`，其中`?:`表示非捕获组，即让小括号里的内容不在结果分组中返回，如果无所谓去掉`?:`即可。如果只想匹配一个正确格式的 JSON 对象字符串，到这里已经结束了。在 Javascript 中，JSON 对象的键名支持单引号引用，也支持数字键名，再加上这两个规则。

<span id="Text3" hidden>{ 'key1': 1, 'key2': "value2" }</span>

<span id="Final3" copy-text="正则表达式已复制。" class="regex">^\s*\{\s\*(?:"[^"]\*"\s\*:[^{}]\*?|'[^']'"\s\*:[^{}]\*?|\d+\s\*:[^{}]\*?|)\}\s\*$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Final3"> 复制</button> <button class="small-text-button gray-button" href="/r?text=$(#Text3)%&regex=$(#Final3)%">试试</button>

好吧，搞这么麻烦，其实在 Javascript 里使用`JSON.parse(str)`或`eval('(' + str + ')')`尝试转换一下就好。只是提供一个解决问题的思路。

（未完待续）