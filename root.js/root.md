# root.js 基础库

**root.js** 是整个标签库的基础，标签库的扩展标签和自定义标签都是基于 root.js 进行开发。root.js 除了基本的元素扩展外，对字符串、数组等数据类型进行了扩展，还提供了很多实用的全局方法。也提供了方便组件开发的方法，如初始化属性和定义事件等。

## HTMLElement 扩展

root.js 为 HTML 中的所有原生标签增加了一些通用的属性和方法。

扩展的属性有：

* `bottom` 获取或设置元素距文档底部的边距，单位像素。
* `css` 只读属性，获取元素上已经设置的所有 CSS 样式，与`style`属性类似，但是`css`属性可以获取定义在 CSS 类中的样式。
* `debug` 是否启用调试，启用后支持这个属性的组件在开发时会在控制台打印更多信息。
* `first` 功能同`firstElementChild`，指向第一个子元素。
* `height` 获取或设置元素的高度，单位像素。INPUT 输入框需要使用原生属性`offsetHeight`。
* `hidden` 获取或设置的是否隐藏，[布尔属性](/root.js/boolean.md)。本身是原生属性，已重写。现在设置为`false`时同时考虑元素的`display`、`visibility`和`opacity`等影响元素可见性的属性。
* `html` 功能同`innerHTML`，得到元素内的所有 HTML。
* `icon` 获取或设置元素的图标，支持 Iconfont 和 图片，图标显示为元素的第一个子节点。
* `last` 功能同`lastElementChild`，指向最后一个子元素。
* `left` 获取或设置元素距文档左侧的边距，单位像素。
* `next` 功能同`nextElementSibling`，指向下一个同胞元素。
* `parent` 功能同`parentNode`，指向其父级元素。
* `right` 获取或设置元素距文档右侧的边距，单位像素，单位像素。
* `text` 功能类似`textContent`，得到元素的文本内容，优先顺序为`text`属性、子标签`<text>`、`textContent`。
* `top` 获取或设置元素距文档顶部的边距。
* `visible` [布尔属性](/root.js/boolean.md)，获取或设置元素可见性，与`hidden`属性相对。
* `width` 获取或设置元素的宽度，单位像素。INPUT 输入框需要使用原生属性`offsetWidth`。

扩展的方法有：

* `on(eventNames, func)` 绑定一个或多个事件到元素上，参数`eventNames`支持多个事件名，由逗号分隔开。
* `dispatch(eventName, ...args)` 执行一个自定义扩展事件。
* `hide()` 隐藏当前元素。
* `show()` 让当前元素可见。
* `insertBeforeBegin(element)` 在元素开始标签之前插入另一个元素或 HTML，`insertAdjacentHTML('beforeBegin', html)` 和 `insertAdjacentElement('beforeBegin', element)`的合体形态。
* `insertBeforeEnd(element)` 在元素结束标签之前插入另一个元素或 HTML，类似于`appendChild(element)`但支持 HTML 字符串。
* `insertAfterBegin(element)` 在元素开始标签之后插入另一个元素或 HTML。
* `insertAfterEnd(element)` 在元素结束标签之后插入另一个元素或 HTML。
* `get(attr)` 获取某一个属性的值。
* `getAttribute(attr)` 重写了原生方法，现在支持检测 Camel 和分隔符两种格式，如`allow-empty`和`allowempty`。
* `$(o)` 同`querySelector(o)`，在元素内选择符合指定 CSS 选择器的第一个子元素，找不到则返回`null`。
* `$child(o)` 只选择下一级子元素。
* `$previous(o)` 选择上一个符合条件的同胞元素。
* `$next(o)` 选择下一个符合条件的同胞元素。
* `$$(o)` 同`querySelectorAll(o)`，在元素内选择符合指定 CSS 选择器的所有子元素，返回一个数组，找不到则返回一个空数组。
* `$$children(o)` 只选择下一级子元素，找不到返回空数组。
* `$$previousAll(o, toElement)` 选择当前元素之前的所有符合条件的同胞元素。`toElement`表示终点元素，即取当前元素和终点元素之间符合条件的所有元素，不设置表示取所有符合条件的元素。
* `$$nextAll(o, toElement)` 选择当前元素之后的所有符合条件的同胞元素。`toElement`表示终点元素，即取当前元素和终点元素之间符合条件的所有元素，不设置表示取所有符合条件的元素。
* `set(attr, value)` 设置某个属性的值，用得比较多，特别是在[精简事件表达式](/root.js/event.md)中。属性名支持 Camel 和连字符格式，子属性通过分号`:`隔开。例如`input.set('value', 'hello')`或`span.set('style:font-size', '1rem')`。
* `setBootom(bottom)` 设置底边距，单位像素。
* `setClass(className)` 设置样式类。
* `setIf(test, attr, value)` 当属性`attr`的值为`test`参数指定的值时才设置`attr`属性的值为`value`。
* `addClass(...classNames)` 在 ClassList 中添加样式类，支持一次添加多个类。
* `removeClass(...classNames)` 从 ClassList 中移除样式类，支持移除多个类。
* `setHeight(height)` 设置高度，单位像素。
* `setHTML(html)` 设置 innerHTML。
* `setIcon(icon)` 设置图标，见`icon`属性。
* `setLeft(left)` 设置左边距，单位像素。
* `setRight(right)` 设置右边距，单位像素。
* `setStyle(name, value)` 设置某个样式。
* `setStyles(styles)` 设置多个样式，`styles`参数传递一个对象。
* `setText(text)` 设置`textContent`。
* `setTop(top)` 设置顶边距，单位像素。
* `setWidth(width)` 设置宽度，单位像素。
* `callout(message, position = 'up', seconds = 0)` 在当前元素的某个位置`position`显示 Callout，内容为`message`，`seconds`为`0`表示一直显示，直到被点击。请参阅本文下方关于 Callout 的内容。

链接编码支持扩展方法有两个：

* `do(func)` 执行一个函数，方便链式操作，见下面示例。
* `if(func)` 如果条件成立，则返回元素自身，否则返回`null`。


所有不返回值的方法都支持链式操作，让代码写得更优雅。

```javascript
this.container.$$('tr[week=v' + week + ']')
    .map(row => row.setClass(this.weekFocusRowClass)
                    .first
                    .setClass(this.weekOfYearFocusClass)
                    .parent
    )
    .first
    .if(row => row.nodeName == 'TR')
    ?.do(row => { //row 表示当前元素
        this.$week = week;
        this.$startDate = row.cells[1].get('value');
        this.$endDate = row.cells[7].get('value');

        this.container.set('week', this.$week)
                      .set('startDate', this.$startDate)
                      .set('endDate', this.$endDate);
    });

calendar.container
    .$('div[sign=CALENDAR-MONTH-SWITCH]')
    .setStyle('visibility', 'visible')
    .show()
    .setLeft(target.left + target.width / 2 - frame.width / 2)
    .setTop(body.top - 8)
    .fadeIn();

```

## Document 全局事件

root.js 为 Document 增加了三个事件，按照触发顺序依次为：

* `onready` 当页面渲染完成之后执行，在`window.onload`事件之前。
* `onpost` 当 [Model 加载数据](/root.js/model.md)完成之后执行。
* `onload` 当页面加载完成之后触发，即 onpost 事件都执行完成，代替`window.onload`事件。

一般情况下使用`onload`事件多一些，而且建议使用`document.onload`事件代替`window.onload`事件。

```javascript
    document.on('load', function() {
        //...
    });
```

三个事件有分别对应的[服务器端事件](/root.js/server.md)，即`onready+`, `onpost+`和`onload+`。这几个服务器端主要用于一次向后端发送数据，写在 BODY 标签上。如果感觉把接口地址或 PQL 语句写在 BODY 标签上有些突兀，可以使用 [IMG 标签](/root.js/image.md)的`onerror+`事件来执行相应的操作。

## Array 数组扩展

扩展属性有：

* `first` 返回数组的第一个元素。
* `last` 返回数组的最后一个元素。

扩展方法有：

* `asc(p)` 升序排序原数组。如果是对象数组，可按照对象名称为`p`的属性值进行排序。
* `avg(p)` 取均值。如果是对象数组，可按照对象名称为`p`的属性值计算。
* `desc(p)` 降序排序原数组。如果是对象数组，可按照计算对象名称为`p`的属性值进行排序。
* `distinct()` 将数组的元素进行排重，还是返回数组。
* `do(func)` 用于链式操作。
* `if(func)` 用于链式操作。
* `intersect(array)` 只保留两个数组都有的内容，即取交集。
* `max(p)` 获取数组中的最大值。如果是对象数组，可计算对象名称为`p`的属性取最大值。
* `min(p)` 获取数组中的最小值，如果是对象数组，可计算对象名称为`p`的属性取最小值。
* `minus(array)` 从原数组中删除`array`中包含的所有内容，即取差集。
* `one()` 如果数组只有一项，则返回这一项，否则返回当前数组。
* `repeat(t)` 将`t`个原数组拼接，即重复`t`次。
* `sum(p)` 获取数组各项累加的结果。如果是对象数组，可计算对象名称为`p`的属性取合计值。
* `toObject(fun)`根据指定逻辑将数据转为对象。
* `toSet()` 将数组转化为 Set 对象，元素会进行排重。
* `union(array)` 连接另外一个数组并排重，即取合集，仅连接不排重可使用`concat(array)`方法。

## String 字符串扩展

* `$()` 将字符串做为选择器的参数执行，如`'#id'.$()`等价于`$('#id')`。
* `$$()` `str.$$()`等价于`$$(str)`。
* `$p(element, data)` [Express 字符串](/root.js/express.md)解析方法，可传入当前元素或对象。不建议直接调用。
* `trimPlus(char1, char2)` 移除字符串两端的指定字符，如移除左右括号 `str.$trim('(', ')')`。也可以指定一个参数，表示左右移除相同的内容。不指定参数时可以移除中文空格。
* `decode()` 进行 HTML 解码。
* `divide(other)` 使用当前字符串把另一个字符串拆分成数组，`split()`方法的反操作。
* `do(str => func)` 执行函数`fun`并返回当前字符串，`str`指向当前字符串，方便链式操作。
* `drop(length)` 从字符串左边开始删除指定长度的内容，如`'abcd'.drop(1)`结果为`'bcd'`。也接受字符串参数，如果字符串以指定参数的字符串开头则删除，如`'onclick'.drop('on')`结果为`click`。
* `dropRight(length)` 从字符串右边开始删除指定长度的内容，如`'abcd'.dropRight(1) = 'abc'`。也接受字符串参数，从右侧删除指定的字符串。
* `eval(obj, data)` 对字母串求值，不传递`obj`时功能同全局函数`eval(str)`；传递`obj`时，将会将字符串转成函数并应用到`obj`上，`data`是函数的可选参数。
* `encode()` 进行 HTML 编码。
* `encodeURIComponent(sure = true)` 进行 URL 地址编码，如果参数`sure`设置为`false`则不进行编码。
* `fill(...elements)` 将字符串内容填充到指定元素内。
* `if(exp)` 接受一个函数或字符串表达式，如果满足条件则返回自身，否则返回`null`，方便配合可选链进行链式操作。
* `ifEmpty(other)` 若原字符串为空则返回`other`，若不为空则返回原字符串。
* `joint(left, right)` 使用`left`和`right`包围当前字符串，`right`可以不设置。
* `prefix(str)` 为字符串增加前缀，如`click.prefix('on')`结果为`'onclick'`。
* `shuffle(digit)` 将字符串重新打乱。如果设置了`digit`，则只返回`digit`位数的字符串。例如`'123'.shuffe()`可能返回`231`，每一位只取一次，不会重复；`'123'.shuffle(2)`可能返回`13`或`22`，可能会重复。
* `suffix(str)` 为字符串增加后缀。
* `take(length)` 从原字符串开头开始截取指定长度并返回，如`'abcd'.take(3) = 'abc'`。
* `takeRight(length)` 获取原字符串从尾部开始到指定长度的内容，如`'abcd'.takeRight(3) = 'bcd'`。
* `takeAfter(value)` 从字符串中获取第一次出现`value`之后的内容，如`'abbcd'.takeAfter(b) = 'bcd'`。
* `takeAfterLast(value)` 从字符串中获取之后一次出现`value`之后的内容，如`'abbcd'.takeAfter(b) = 'cd'`。
* `takeBefore(value)` 从字符串中获取第一次出现`value`之前的内容，如`'abcd'.takeBefore(c) = 'ab'`。
* `takeBeforeLast(value)` 从字符串中获取最后一次出现`value`之前的内容，如`'abcddcba'.takeBeforeLast('c') = 'abcdd'`。

一些字符串转化方法：

* `recognize()` 自动识别字符串的类型并转换，可识别布尔值、整数和小数，如`'yes'.recognize()`返回`true`，`'23'.recognize()`返回`23`。
* `toInt(defaultValue = 0)`  将字符串转化为整数，比`parseInt`方法更智能。
* `toFloat(defaultValue = 0)`  将字符串转化为小数，比`parseFloat`方法更智能。
* `toBoolean(defaultValue = false)` 将字符串转化为布尔值，会把`yes`, `1`, `true`, `ok`, `on`识别为`true`，会把`no`, `0`, `false`, `cancel`, `off`识别为`false`，详细规则见[布尔属性](/root.js/boolean.md)。
* `toArray(delimiter = ',')` 将字符串转化转化数组，类似于`split`方法，也可以把`'[1,2,3,4]'`解析成数组。
* `toMap(delimiter = '&', separator = '=')` 将字符串转化为 Object 结构，比较常用的是地址参数，也可转换 Json 格式的字符串。
* `toObject()` 将对象字符串转成对象。
* `toCamel()` 将分隔符形式的字符串转驼峰格式，如`'mine-type'.toCamel()`结果是`mineType`。
* `toPascal()` 将分隔符形式的字符串转 Pascal 格式，如`'mine-type'.toPascal()`结果是`MineType`。
* `toHyphen()` 将驼峰格式的字符串转分隔符格式，如`'mineType'.toHyphen()`结果是`mine-type`。

一些字符串判断，全部为属性：

* `isArrayString` 判断字符串是否可转为数组。
* `isDateString` 判断字符串是否为日期。
* `isDateTimeString` 判断字符串是否为日期时间。
* `isEmpty` 判断字符串是否为空。* `isObjectString()` 判断字符串是否是可转为对象。
* `isImageURL` 是否图片地址字符串。
* `isIntegerString` 判断字符串是否可转为 Int 类型。
* `isNumberString` 判断字符串是否可转为 Number 类型。
* `isObjectString` 判断字符串是否是对象字符串。
* `isTimeString` 判断字符串是否为时间。

字符串扩展属性

* `unicodeLength` 可以把中文字符识别为`2`个位置，`'中文'.unicodeLength`结果为`4`

静态方法

* `String.shuffle(digit = 7)` 返回一个由大小写字母和数字构成的随机字符串。
* `String.generateGUID()` 生成一个由时间戳和 11 位随机字符串构成的 GUID。

## RegExp 正则表达式扩展

* `findFirstIn(str)` 查找字符串`str`中的第一个匹配字符串，找不到时返回`null`。
* `findFirstMatchIn(str)` 查找字符串`str`中的第一个匹配，找不到时返回`null`。与`exec()`方法不同，`exec`当有`g`修饰符时多次执行时会继续寻找下一个匹配。`findFirstMathIn`方法永远只找第一个匹配。
* `findAllIn(str)` 查找字符串`str`中的所有匹配，返回所有匹配字符串的数组。
* `findAllMathIn(str)` 查找字符中`str`中的所有匹配，返回所有匹配的数组。

## Number 数字扩展

* `floor(n = 0)` 去除多余的小数部分，保留`n`位小数。
* `ifNegative(v)` 判断数字是否为负数，为负数则返回`v`。
* `ifNotZero(v)` 判断数字是否不为`0`，不为`0`则返回`v`。
* `ifPositive(v)` 判断数字是否为正数，为正数则返回`v`。
* `ifZero(v)` 判断数字是否为`0`，为`0`则返回`v`。
* `kilo()` 为数字增加千分符，返回字符串。如`12004.kilo()`结果是`'12,004'`
* `max(other)` 和另一个数字进行比较，返回大的那一个，同`Math.max(this, other)`。
* `min(other)` 和另一个数字进行比较，返回小的那一个，同`Math.min(this, other)`。
* `opposite(condition)` 根据条件判断是否返回相对数，如果`condition`为`null`或`true`，则返回数字的相对数，否则返回自身。`condition`支持各种布尔值运算，如字符串`1 == 2`。
* `percent(digits = 2)` 将数字转换为百分数字符串，默认保留两位小数。如`0.65487.toPercent()`结果是`65.49%`
* `round(n = 0)` 将数字四舍五入，保留`n`位小数。
* `toTimeSpan(max)` 将秒单位的数字转成易读的时间间隔值，如`1d24h`。

## 全局属性

一些关于文档的全局属性。

* `$root.lang` 当前浏览器的语言，如中文为`zh`。
* `$root.mobile` 当前是否在移动设备上运行，布尔值。反过来就是在 PC 设备上运行。
* `$root.scrollTop` 获取或设置滚动条当前的纵向位置。
* `$root.scrollLeft` 获取或设置滚动条当前的横向位置。
* `$root.visibleWidth` 获取当前可视范围的宽度，不建议使用。
* `$root.visibleHeight` 获取当前可视范围的高度, 在iframe中时不准，不在iframe时也不准，不建议使用。
* `$root.documentWidth` 获取文档的宽度。
* `$root.documentHeight` 获取文档的高度。
$ `$else` 记录`if`方法前的值，例如元素标签或字符串都有`if`方法，方便进行链接编码。
    例如`$('#id').if(e => e.nodeName == 'DIV').addClass('className') ?? $else.remove()`中的`$else`表示`$('#id')`。

## 全局方法

解析地址字符串：

* `$query.get(name)` 获取地址中名称为`name`的参数。
* `$query.has(name)` 判断地址中是否包含名称为`name`的参数。

Cookie 操作

* `$cookie.get(name)` 获取名为`name`的 Cookie 的值。
* `$cookie.set(name, value)` 设置名为`name`的 Cookie 的值为`value`。
* `$cookie.has(name)` 判断 Cookie 中是否包含`name`。

创建元素：

* `$create(tag, properties, styles, attributes)` 创建一个标签元素，返回创建的元素。
    + `tag` 标签名，如`DIV`
    + `properties` 属性列表，必须是一个 Object，如`{ "className": "banner", "innerHTML": "Hello World" }`
    + `styles` 样式列表，必须是一个 Object，如`{ "backgroundColor": "#FF0000", "font-size": "2rem" }`
    + `attributes` 自定义属性列表，可以是一个 Object，如`{ "sign": "TEST" }`。也可以是一个元素，即将元素的所有属性复制给要创建的元素。

全局样式类

* `$root.appendClass(classText)` 为整个页面添加文本样式类，如`classText`可以是 `span { font-size: 14px }`。

全局转化方法：<a id="parse"></a>

* `$parseString(value, defaultValue = '')` 尝试将数值转成字符串。
* `$parseInt(value, defaultValue = 0)`  尝试将数值转成整数。
* `$parseFloat(value, defaultValue = 0)` 尝试将数值转成小数。
* `$parseBoolean(value, defaultValue = false)` 尝试将数值转成布尔值，否则返回默认值。根据`value`的类型不同，有不同的判断条件。分别说明：
    + `null`或`undefined` 返回默认值；
    + 数字，是否大于`0`；
    + 字符串，会识别为`true`的字符串有`true`、`yes`、`selected`、`disabled`、`enabled`、`1`、`ok`、`on`或`空字符串`，会识别为`false`的字符串有`null`、`undefined`、`false`、`no`、`0`、`cancel`或`off`，不区分大小写。其他值`eval`计算之后再进行识别，计算失败在控制台返回异常信息也返回默认值；详见[布尔属性](/root.js/boolean.md)。
    + 数组，长度是否大于`0`；
    + 对象，是否有属性或数据项；
    + 类实例，是否有`length`或`size`属性且大于`0`；
    + 其他，返回默认值。
* `$parseArray(value, defaultValue = [])` 尝试将数值转成数组。
* `$parseEmpty(value, defaultValue = true)` 检查给定值是否为空，一定程度上可以理解为是`$parseBoolean`判断的逆操作：
    + `null`或`undefined`，返回默认值；
    + 字符串，是否为空；
    + 数组，长度是否为`0`；
    + 对象，是否无属性或空数据项；
    + 布尔值，是否为`false`；
    + 数字，是否小于`0`；
    + 其他，返回默认值。
* `$parseZero(value, defaultValue = 1)` 检查给定值是否为`0`，如果不是数字，则尝试转成数字。
    + `null`或`undefined`，返回默认值；
    + 字符串，尝试转成数字再判断，如果不能转成数字，则使用默认值；
    + 数组，长度是否为`0`；
    + 对象，是否无属性或空数据项；
    + 布尔值，是否为`false`；
    + 其他，返回默认值。

其他更多：

* `Math.randomNext(begin, end)` 随机取`begin`到`end`之间的整数。

## 选择器

可用的选择器如下：

* `$(o)`或`$s(o)` 获取符合条件的第一个标签或元素，如`$s('a')`返回页面上的第一个链接。其中`s`表示`single`，这个方法可选择原生标签和自定义标签，且自定义标签会被优先选择。可通过[全局设置](/root.js/config.md)修改为其他名称。
* `$$(...o)`或`$a(...o)` 获取符合条件的全部标签或元素，支持输入多个参数，如`$a('p', 'div')`返回页面上所有 P 标签和 DIV 标签。其中`a`表示`all`，这个方法可选择原生标签和自定义标签。可通过[全局设置](/root.js/config.md)修改为其他名称。


```javascript
$s('#Title').innerHTML = 'HELLO WORLD!';
$a('#tag1,#tag2').forEach(...);
```

## Ajax 相关方法

Ajax 用来从接口调取数据，以下 4 个方法分别对应 4 种不同的 Method，不支持跨域。

* `$GET(url, path = '/')`
* `$POST(url, params = '', path = '/')`
* `$PUT(url, params = '', path = '/')`
* `$DELETE(url, path = '/')`

这 4 个方法中，`url`表示要请示的地址；`params`表示要发送的数据或参数，格式与地址字符串相同；`url`和`params`均支持 [Express 字符串](/root.js/express.md)；`path`表示获取数据成功后解析数据的 JsonPath。

* `sync(enabled)` 设置请求是同步还是异步请求，默认为异步，只有需要设置为同步时才需要使用这个方法。
* `on('event', func)` 设置请求回调方法，`event`支持`send`、`error`、`success`和`complete`，分别表示发送时、失败时、成功时、完成时。
* `send()` 执行发送请示。

完整的示例和函数参数如下：

```javascript
    $POST('/api/user', 'name=Tom&age=18', '/data')
        .on('send', function(url, params) {
            //url 和 params 是解析后的值，一般用于调试
        })
        .on('error', function(status, statusText) {
            //status 是状态码, statusText 是状态文本
        })
        .on('complete', function() {
            //无论成功还是失败，都执行这个函数
        })
        .on('send', function(data) {
            //data 表示请求成功后获得的数据
        }).send();
```

现在已经有了更强大的请求方法：

* `$cogo(todo, element)` 可以运行 PQL 语句或请示接口（支持跨域），但需要后端配合。
    + `todo`表示要运行的 PQL 语句或要请求的接口，支持 [Express 字符串](/root.js/express.md)，属性格式详见[与数据相关的属性](/root.js/data.md)与 PQL 语句或接口相关的说明。
    + `element`表示发起请示的元素或对象。
    + `$cogo`返回一个 *Promise* 对象。

示例：

```javascript
$cogo('select * from table1')
    .then(data => {
        // data 表示返回的数据
    });

$cogo('post:http://www.domain.com/api/student?name=Tom -> /data')
    .then(data => {
        // ...
    })
```

## JSON 扩展

root.js 提供了对 JSON 全局对象进行了一些扩展，以方便 Json 数据类型的解析，全部为静态方法。

* `JSON.stringifo(o)` 尝试将一个 Object 对象或非字符串类型转成字符串，当参数`o`不为字符串，对`o`进行字符串化，否则仍返回`o`。可以说是原方法`stringify`的升级版本。
* `JSON.eval(str)` 尝试将字符串直接转成 Object 对象，可以理解为是原方法`parse`的升级版本。
* `JSON.find(object, path = '/')` 按 JsonPath 查找 Object 对象的内容。


Json 构造函数和方法有：

* `Json(strOrObject)` 构造函数参数可传入字符串或对象，并尝试转成 Json 对象。
* `find(path)` 根据 JsonPath 查找数据。
* `toString()` 将 Json 对象转成字符串。

静态方法有：



## Callout

Callout 可以在指定元素的位置显示指定的文字内容，一些用于提示。在标签库中与其他标签配合使用。

```javascript
Callout('message').position(reference, pos).offset(x, y).show(seconds);
```

其中`message`内容可定义；`reference`为参考元素，表示在哪个元素的附近显示 Callout；`pos`为位置项，可选`up`、`down`、`left`、`right`，默认为`up`；`offset`设置偏移坐标；`seconds`设置多少秒后自动关闭，不设置则一直显示，直到被点击。一个页面上只能显示一个 Callout，点击 Callout 会自动隐藏。


## 与组件编程相关内容

root.js 还包含与组件编程相关的操作，以方便开发组件。

### 扩展原生标签

root.js 提供了扩展原生标签的方法。

```javascript
$enhance(HTMLButtonElement.prototype)
    .defineProperties({
        enabledClass: 'normal-button blue-button',
        disabledClass: 'normal-button optional-button',
        actionText: '',
        action: '', //要执行的 PQL 语句或要请求的接口
        'text': {
            get () {            
                return this.innerHTML;
            },
            set (text) {
                this.innerHTML = text;
            }
        }
    })
    .defineEvents('onclick+', 'onclick-disabled');
```

以上代码中扩展了 Button 原生标签，各方法分别说明如下：

* `$enhance()` 传递要扩展的标签原型对象，也可传递其他对象，这里只能是对象。
* `defineProperties()` 添加可在标签上定义的扩展属性，可以覆盖原生属性，支持`getter`和`setter`设置。
* `defineEvents()` 添加自定义事件，支持客户端事件和服务器端事件。

还可以通过`prototype`定义更多原型属性，不过这样定义的属性不支持`getter`和`setter`。扩展方法也可以在原型对象上定义，如下例：

```javascript
HTMLButtonElement.prototype.relatived = 0;
HTMLButtonElement.prototype.initailize = function() {
    //...
};
```

## 自定义属性或自动执行的功能

除上面介绍的所有开发者可用的方法外，还有一些自动执行的功能。

* 自动调整页面布局 DIV 的高度以填满整个浏览器，需要在 BODY 标签中设置`self-adaption`属性。
    
    ```html
    <body self-adaption="#CatalogDiv,#WorkFrame">
    ```

* 锁定 DIV 和 TABLE，一般用于导航和表格表头固定，需要在 DIV 或 TABLE 上添加`fixed`属性。

    ```html
    <div id="Nav" fixed="yes">...</div>
    ```

* 自动调整 IFRAME 的高度，需要在 IFRAME 嵌入的页面的 BODY 标签里添加`iframe`属性，告诉页面它在哪个 IFRAME 里。

    ```html
    <body ifram="#WorkFrame">
    ```


---
参考链接

* [Express 字符串](/root.js/express.md)
* [布尔属性](/root.js/boolean.md)
* [服务器端事件](/root.js/server.md)
* [精简事件表达式](/root.js/event.md)