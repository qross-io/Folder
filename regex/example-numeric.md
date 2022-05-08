<link href='/regex.css' rel='stylesheet' type='text/css'>

# 正则表达式示例 - 数字


## 整数

整数形式有好多种，是正则中用得非常多的场景。1 位数字用正则表示为`\d`或`[0-9]`，指定某几位数字如限定 1 到 5 就是`[1-5]`，限定偶数就是`[02468]`，限定 1、2、3、6、7、9 就是`[1-3679]`等等。下面示例中有时省略开始符号`^`和结束符号`$`，就看是否完全匹配整个字符串，如果完全匹配就加上，如果不完全匹配就不加。

### 固定位数的整数

<span id="Number1" class="regex" copy-text="正则表达式已复制。">\b\d{6}\b</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number1">复制</button>
<button class="small-text-button gray-button" href="/r?text=(您的验证码是 532618，请不要告诉他人。如果验证失败，可回复短信 Y 至 10663789003778690045 重新验证。)%&regex=$(#Number1)%">试试</button>

其中`{6}`表示重复 6 次，即要求数字必须是 6 位。`\b`表示边界，就是说匹配结果的两端不能再是数字。比较常见的 6 位数字是验证码，这个表达式允许以 0 开始。

<span id="Number2" class="regex" copy-text="正则表达式已复制。">3\d{5}</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number2">复制</button>
<button class="small-text-button gray-button" href="/r?text=352618&regex=$(#Number2)%">试试</button>

这样表示必须以数字`3`开始，加上后面 5 位，一共还是 6 位。省略了边界符`\b`，看情况是否需要添加，以下均同。

<span id="Number3" class="regex" copy-text="正则表达式已复制。">[1-9]\d{4}</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number3">复制</button>
<button class="small-text-button gray-button" href="/r?text=26187&regex=$(#Number3)%">试试</button>

这样表示 5 位数字，且第 1 位不能是`0`。

### 从 m 到 n 的整数

<span id="Number4" class="regex" copy-text="正则表达式已复制。">^[1-9]\d{0,3}$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number4">复制</button>
<button class="small-text-button gray-button" href="/r?text=6288&regex=$(#Number4)%">试试</button>

上例用户输入必须是`1`到`9999`之间的整数。`^[1-9]`表示字符串开头必须是`1`到`9`之间的数字，`{0,3}`表示前面的符号匹最少`0`次，最多`3`次，当`0`次时结果就是个位数，`3`次时就是千位数。如果范围是从`0`开始，那么就是下面的表达式。

<span id="Number5" class="regex" copy-text="正则表达式已复制。">^\d{0,4}$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number5">复制</button>
<button class="small-text-button gray-button" href="/r?text=0&regex=$(#Number5)%">试试</button>

如果不是`999`这样的数，如果是`853`怎么办？比如，要求数字在`42`到`853`之间，用正则表达式验证起来就会比较闹心，不过也有办法。

<span id="Number6" class="regex" copy-text="正则表达式已复制。">^(4[0-2]|[1-7][0-9]{2}|8[0-4][0-9]|85[0-3])$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number6">复制</button>
<button class="small-text-button gray-button" href="/r?text=577&regex=$(#Number6)%">试试</button>

就是要把所有的情况都列出来，简单粗暴。其中`4[0-2]`表示`40`到`42`，`[1-7][0-9]{2}`或`[1-7][0-9][0-9]`表示`100`到`799`，`8[0-4][0-9]`表示`800`到`849`，`85[0-3]`表示`850`到`853`。

### 奇偶数

<span id="Number7" class="regex" copy-text="正则表达式已复制。">^([13579]|[1-9]\d{0,2}[13579])$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number7">复制</button>
<button class="small-text-button gray-button" href="/r?regex=$(#Number7)%&text=6287">试试</button>

-- 20 --

<table class="regex-table" cellspacing="2" cellpadding="2">
    <tr>
        <td>^</td>
        <td>(</td>
        <td>[13579]</td>
        <td>|</td>
        <td>[1-9]</td>
        <td>\d{0,2}</td>
        <td>[13579]</td>
        <td>)</td>
        <td>$</td>
    </tr>
    <tr>
        <td>&nbsp;</td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
        <td>&nbsp;</td>
    </tr>
</table>

-- 10 --

这个表达式表示输入必须是`1`到`9999`之间的奇数，1 位数和 2 位以上的数分别表示，各部分分别解释如下：

* 第 1 项和第 7 项圆括号`(`和`)`用于分组，这里主要目的是为了竖线符号`|`服务。
* 第 2 项`[13579]`表示 1 位数字，只能是单数。
* 第 5，6，7 项一起表示 2 位以上的数。第 5 项`[1-9]`表示数字不能以`0`开头，第 6 顶`\d{0,2}`表示0 到 2 位数字，第 7 项`[13579]`表示数字必须以单数结尾。
* 竖线符号`|`表示或，即第 2 项和第 5，6，7 项（表示 2 位以上的数）之间是或的关系。即要么匹配 1 位数，要么匹配 2 位以上的数。


<span id="Number8" class="regex" copy-text="正则表达式已复制。">^([02468]|[1-9]\d{0,2}[02468])$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number8">复制</button>
<button class="small-text-button gray-button" href="/r?text=6288&regex=$(#Number8)%">试试</button>

上例表示从`0`到`9998`之是的偶数，基本与奇数的表达式相同。如果要求大于`0`的偶数，那么就把前面的`[02468]`改成`[2468]`就可以了。

注意正则表达式不能判断质数和合数，只能通过程序来判断。

## 负整数

### -n 到 n 的整数

比如从负`999`到正`999`，不允许`-0`这样的格式。

<span id="Number9" class="regex" copy-text="正则表达式已复制。">^(-[1-9][0-9]{0,2}|[0-9]{0,3})$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number9">复制</button>
<button class="small-text-button gray-button" href="/r?text=-372&regex=$(#Number9)%">试试</button>

分负整数和正整数两部分来表达，左面`-[1-9][0-9]{0,2}`表示从`-999`到`-1`的负整数，右面`[0-9]{0,3}`表示从`0`到`999`之间的整数。

### -n 到 0 的整数

例如从`-999`到`0`那么表达式是：

<span id="Number10" class="regex" copy-text="正则表达式已复制。">^(-[1-9][0-9]{0,2}|0)$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number10">复制</button> <button class="small-text-button gray-button" href="/r?text=-372&regex=$(#Number10)%">试试</button>

### `-m`到`-n`的整数

全负数范围，例如`-99`到`-8`的负整数可以表达为：

<span id="Number11" class="regex" copy-text="正则表达式已复制。">^-([89]|[1-9][0-9])$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number11">复制</button> <button class="small-text-button gray-button" href="/r?text=-372&regex=$(#Number11)%">试试</button>

注意负号`-`这次放到了小括号的外面。

## 小数

### 小数点后保留 n 位

整数部分为`0`，小数点后面最多有两位，即最小为`0.01`，最大为`0.99`，不允许为`0`、`0.0`或`0.00`。

<span id="Number12" class="regex" copy-text="正则表达式已复制。">^0\.([1-9]|[0-9][1-9])$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number12">复制</button> <button class="small-text-button gray-button" href="/r?text=0.03&regex=$(#Number12)%">试试</button>

其中`\.`表示小数点，因为圆点在正则表达式中表示所有字符，所以要加上转义符号。竖线`|`左面`[1-9]`表示`0.1`-`0.9`这 9 个数字，竖线`|`右面`[0-9][1-9]`表示`0.01`-`0.09`、`0.11`-`0.19`、`0.21-0.29`等等。

### 整数和小数混合

如果既允许小数又允许整数，比如大于等于`0`且小于`10`的整数和 1 位小数，不允许`1.0`这样的表示法，那么表达式可以是

<span id="Number13" class="regex" copy-text="正则表达式已复制。">^\d(\.[1-9])?$</span> &nbsp; &nbsp; <button class="small-text-button gray-button" onclick-="copy: #Number13">复制</button> <button class="small-text-button gray-button" href="/r?text=6.9&regex=$(#Number13)%">试试</button>

其中`\.[1-9]`表示小数部分，问号`?`表示前面的内容有或者没有，这里问号`?`前面是小括号包围的部分。小括号部分没有时即为整数，有时即为小数。

（本文完）

