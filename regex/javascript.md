# Javascript 中的正则表达式

这里仅介绍 Javascript 中使用正则表达式的方法和基础知识，示例和技巧见 [Javascript 正则表达式操作示例](/regex/javascript-example.md)。

```javascript
let s = 'Hello World';
//查找目标字符中是否存在 h 和 l 字母，并忽略大小写
if (/[hl]/i.test(s)) {
    console.log('Found.');
}
```

上例中，`/[hl]/i`部分即为正则表达式，使用正则表达式的方法来对字符串进行匹配和其他操作。

## 正则表达式类

一般使用`/`包围的方式来声明正则表达式，`[hl]`表示只匹配`h`和`l`两个字母，`i`是一个修饰符，表示忽略大上写。再举个例子。

```javascript
let s = 'Hello World, today is 2021-4-29.';
let x = s.replace(/\d/g, 'x');
```

上例中将字符串`s`中的所有数字替换成`x`，其中`\d`表示匹配数字，`g`表示查找所有匹配。

```javascript
let r = new RegExp('\\d', 'g');
```

也可以使用正则表达式类创建实例，上例变量`r`等同于`/\d/g`。

## 相关方法

* `regex.test(str)` 检查正则表达式是否与字符串匹配。
* `regex.exec(str)` 查找匹配，返回一个数组对象，包含每一次匹配和其他信息。可不断的执行直到`null`为止。
    + 数组索引包含匹配后的分组捕获信息：`0` 表示`group(0)`，即整个匹配；`1`表示`group(1)`，依次类推。
    + `index` 表示匹配在字符串中的位置，索引从 0 开始。
    + `input` 表示输入的字符串。
* `string.match(regex)` 如果`regex`的修饰符不含`g`，作用与上一个方法相同，如果修饰符包含 `g`，那么返回包含所有匹配的数组。
* `string.matchAll(regex)` 参数`regex`必须包含修饰符`g`，返回所有匹配的迭代器对象 RegExpStringIterator。ES 11 新增。
* `string.replace(regex, strToReplace)` 字符串的替换方法，第一个参数可以使用字符串或正则表达式，修饰符不包含`g`则只替换第一项，包含`g`则替换全部匹配。ES 12 新增`replaceAll`方法，可以不用正则表达式就可以进行全部替换了。
* `string.search(strOrRegex)` 查找匹配结果字符串在指定字符串中的位置。
* `string.split(strOrRegex)` 按指定的正则表达式分隔字符串。

## 相关属性

非修饰符属性如下：

* `input` 要匹配的原始字符串。
* `source` 正则表达式的字符串表示。
* `index` 匹配到的字符位于原始字符串的基于 0 的索引值。
* `lastIndex` 下一次匹配开始的位置。

修饰符属性如下：

* `flags` 所有修饰符。
* `ignoreCase` 是否忽略大小写。
* `global` 是否进行全局匹配。
* `multiline` 是否使用了 "m" 标记使正则工作在多行模式。也就是，^ 和 $ 可以匹配字符串中每一行的开始和结束（行是由 \n 或 \r 分割的），而不只是整个输入字符串的最开始和最末尾处。
* `dotAll` 属性表明是否在正则表达式中一起使用`s`修饰符，使得`.`可以匹配任意单个字符，不使用`s`修饰符则`.`不匹配回车和换行符。
* `sticky` 是否使用了`y`修饰符，可以理解为每次使用`exec`匹配时默认添加开始符号`^`。
* `unicode` 是否使用了`u`修改符，表示匹配 Unicode 字符，如中文。

## 修饰符

修饰符修饰符可以理解为正则表达式类的选项，可以直接在声明时指定，也可以在正则表达式对象上通过属性设置。下面 4 条语句等效。

```javascript
let x = /[a-z]/ig;
let x = new RegExp('[a-z]', 'ig');
let x = /[a-z]/; x.ignoreCase = true; x.global = true;
let x = new RegExp('[a-z]'); x.ignoreCase = true; x.global = true;
```

所有修饰符的功能如下：

* `i` 对应属性`ignoreCase`，忽略大小写
* `g` 对应属性`global`，查找字符串的所有匹配。
* `m` 对应属性`multiline`，匹配多行。这个参数的使用比较苛刻，主要用于配合开始符号`^`和结束符号`$`使用。要求待匹配字符串中必须有换行符`\n`，且正则表达式中有开始符号`^`或结束符号`$`，则`m`修饰符才有意义。不使用`m`时，`^`匹配整个字符串的开始，`$`匹配整个字符串的结束；使用`m`时，`^`匹配字符串每一行的开始，`$`匹配字符串每一行的结束。
* `y` 对应属性`sticky`，可以理解为默认添加开始符号`^`，通过正则表达式的`lastIndex`属性可以设置开始匹配位置。ES 6 新增。
* `s` 对应属性`dotAll`，符号`.`默认匹配一行内所有字符，即不匹配回车符号`\r`和`\n`，使用`s`会改变这种情况，即`.`符号匹配所有字符。ES 9 新增，在这之前使用`[\s\S]`匹配任何字符。
* `u` 对应属性`unicode`，匹配 Unicode 字符。`\uxxxx` 查找以十六进制数`xxxx`规定的 Unicode 字符，如中文。

修饰符可以混合使用，如`igs`。在 Java 和 Scala 中，修饰符写在正则表达式中最前面位置，如`(?i)` 不区分大小写，Javascript 不支持这样写。

## 反向引用

使用符号`$n`可以得到最近一次匹配的内容，可使用`$0`到`$9`，表示匹配结果中各分组捕获的值。

```javascript
'04/29/2021'.replace(/(\d{2})\/(\d{2})\/(\d{4})/, '$3-$1-$2')  //返回 2021-04-29
'border-left-color'.replace(/-([a-z])/g, (m, n) => n.toUpperCase()); //返回 borderLeftColor
'borderLeftColor'.replace(/[A-Z]/g, l => '-' + l.toLowerCase()); //返回 border-left-color
```

## 命名捕获组

ES 9 支持对分组进行命名，使用`?&lt;name&gt;`，写在分组内。

```javascript
let x = /(?<month>\d{2})\/(?<day>\d{2})\/(?<year>\d{4})/.exec('04/29/2021');
let y = x.groups.year + '-' + x.groups.month + '-' + x.groups.day; //得到结果 2021-04-09
```


---
参考链接

* [正则表达式测试器](/r)
* [正则表达式语法](/regex/syntax.md)