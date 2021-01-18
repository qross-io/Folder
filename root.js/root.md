# root.js 基础库

**root.js** 是整个组件库的基础，root.js 基础库提供类似 **jQuery** 的功能，但是对细节更容易控制。组件库的其他组件都是基于 root.js 进行开发。root.js 除了基本的选择器功能外，对字符串、数组等数据类型进行了扩展，还提供了很多实用的全局方法。另提供了方便组件开发的方法，如初始化属性和定义事件。

## 选择器和 DOM 操作

最常用的选择器 `$x(...o)`，参数支持CSS选择器字符串、元素对象或元素集合，一般为CSS选择器字符串，返回一个可继续操作的对象`$root`。该方法支持传入多个参数，类似 jQuery 的选择器`$()`。`$x`是 DOM 操作的起点，后续的很多方法都需要先选择元素。

```javascript
$x('#Title').html('HELLO WORLD!');
$x('div').objects.forEach(div => ...);
```

还有其他可用的选择器：

* `$s(o)` 获取符合条件的第一个标签或元素，如`$s('a')`返回页面上的第一个链接。其中`s`表示`single`，这个方法也只可选择原生标签。
* `$a(...o)` 获取符合条件的全部标签或元素，支持输入多个参数，如`$a('p', 'div')`返回页面上所有 P 标签和 DIV 标签。其中`a`表示`all`，这个方法只能选择原生标签。
* `$t(o)` 选择单个自定义标签，传入标签`name`，如 TREEVIEW、CALENDAR等。其中`t`表示`tag`，这个方法只能选择自定义标签。另外每个组件中也提供了选择器方法，如`$tree(name)`。
* `$c(...o)` 获取符合条件的全部自定义标签，支持输入多个参数，如`$t('calendar,clock')`返回页面上所有 CALENDAR 标签和 CLOCK 标签。其中`c`表示`components`，这个方法只能选择自定义标签。


这两个选择器与`$x`选择器不同是，`$x`返回一个可继续操作元素的对象，可以使用系统的很多方法继续进行元素操作；而`$s`和`$a`选择器返回元素本身，需要使用元素本身的方法继续操作元素。

```javascript
$x('#Title').html('HELLO WORLD!');
$s('#Title').innerHTML = 'HELLO WORLD!';
```

上例两条语句实现的功能相同。

### $x 的下一步操作

`$x`选择器返回`$root`对象，这个对象包含很多方法，可对元素进行很多操作。`$root`对象里有一个数组保存已经选择到的所有元素，下文称为`$root`集合。

`$x`大部分方法都返回`$root`对象本身，所以可以多个方法串接到一起进行操作。例如：

```javascript
$x('div').last().css('nav').show();
```

在元素导航之间导航：

* `push(...o)` 可以理解把两次`$x`的结果合在一起。
* `first()` 只留下集合中的第一个元素。
* `last()` 只留下集合中的最后一个元素。
* `pick(i)` 只留下指定索引位置的元素，如果`i`小于`0`则保留第`1`个元素，如果`i`大于总元素数量，则保留最后一个元素。
* `select(...o)` 在当前元素集合的基础上继续选择子元素。
* `parent()` 返回每个集合元素的父元素。
* `prev()` 返回每个集合元素的上一个标签元素（`nodeType == 1`），相当于`previousElementSibling`。
* `next()` 返回每个集合元素的上一个标签元素（`nodeType == 1`），相当于`nextElementSibling`。
* `before()` 返回每个集合元素的上一个元素，包括标签、文本或注释，相当于`previousSibling`。
* `after()` 返回每个集合元素的下一个元素，包括标签、文本或注释，相当于`previousSibling`。
* `children()` 返回每个元素的所有节元素。
* `firstChild()` 返回集合中每个元素的第一个子元素。
* `lastChild()` 返回集合中每个元素的最后一个子元素。
* `nthChild(index)` 返回集合中每个元素的指定索引位置的子元素。

元素基本操作：

* `show(display = true)` 显示集合中的所有元素，当`display`为`false`，则隐藏集合中的所有元素。
* `hide()` 隐藏集合中的所有元素。
* `toggle()` 自动切换隐藏和显示状态，如果是隐藏，则切换成显示；否则亦然。
* `remove()` 从页面上清除集合中所有元素。
* `switch(attr, value1, value2)` 切换元素的属性。
* `style(name, value)` 设置元素的样式。
* `styles(styleObject)` 设置元素的多个样式，参数必须是一个对象，如`{ "color": "#009900", "text-decoration": "underline"}`。
* `stash(attr)` 暂存元素的属性值，可能需要对元素的属性进行修改，并在将来使用元素的旧值。
* `reset(attr)` 恢复元素的属性值。
* `css(name)` 设置元素的样式单 class。
* `swap(css1, css2)` 切换元素的样式单 class。

表单相关操作：

* `enable(enabled = true)` 启用表单元素，如按钮、输入框等。如果`enabled`为`false`，则禁用这个表单元素。
* `disable()` 禁用表单元素。
* `check(checked)` 设置复选框的选中或不选中。
* `incheck()` 半选中复选框。
* `uncheck()` 取消选中指定复选框。
* `tocheck()` 切换复选框的选中状态。

绑定事件：

* `on(eventName, func, useCapture = false, attach = true)` 为元素添加监听事件，可同时绑定函数给多个事件。
    + `eventName` 事件名称，如`onclick`，`onload`等，`on`前缀可以省略
    + `func` 事件函数
    + `useCapture` `false` 在冒泡阶段执行，`true` 在捕获阶段执行
    + `attach` 是附加操作还是赋值操作。附加操作是添加监听，赋值操作可以理解为直接把`func`赋给事件，如`element.onclick = function()...`
* `un(eventName, func, useCapture = false, attach = true)` `on`方法的逆操作。

子元素操作，各参数的意义见`$create()`全局方法：

* `insertFront(tag, properties, styles, attributes)` 在指定元素之前插入按照传入参数生成的元素。
* `insertBehind(tag, properties, styles, attributes)` 在指定元素之后插入按照传入参数生成的元素。
* `insertFirst(tag, properties, styles, attributes)` 在指定元素的第一个子元素之前插入按照传入参数生成的元素。
* `append(tag, properties, styles, attributes)` 在指定元素的最后一个子元素之后插入按照传入参数生成的元素。

与元素属性相关的操作，下面的方法当传参时设置元素相应的属性值，不传参时返回相应的属性值。如果不传参数（获取值时），当集合中有多个元素时，则返回数组。

* `html(code)` 设置或获取元素的`innerHTML`，即所有 HTML 代码。
* `text(text))` 设置或获取元素的`textContent`，即所有文本内容。
* `value(value)` 设置或获取表单元素的`value`。
* `attr(name, value)` 设置或获取元素的属性值，`name`为属性名称。如`$x('#span1').attr('tip', 'Hello!')`
* `left(value)` 设置或获取元素左边框到可视区左边的距离。
* `right(value)` 设置或获取元素右边框到可视区域左边的距离。
* `top(value)` 设置或获取元素顶部到可视区域顶部的距离。
* `bottom(value)` 设置或获取元素底边框到可视区域顶部的距离。
* `width(value)` 设置或获取元素的宽度。
* `height(value)` 设置或获取元素的高度。
* `css(name)` 设置或获取元素的样式单。


选择器基本操作，返回布尔值、数字或字符串：

* `nonEmpty()` 判断`$root`集合是否为空。
* `isEmpty()` 判断`$root`集合是否不为空。
* `size()` 返回`$root`集合的元素个数。
* `is(name)` 判断集合中第一个元素名称是否是`name`，`name`不区分大小写。
* `get(i)` 返回指定索引位置的元素本身，不传参数表示得到第一个元素，当`i`超过索引范围时返回`null`。
* `visible()` 判断第一个元素是否是可见状态。
* `hidden()` 判断第一个元素是否是隐藏状态。

定位相关操作，用于将元素定位到参照元素的附近。参数`reference`表示参考元素，接受元素对象或字符串；`offsetX`和`offsetY`分别为 X 轴和 Y 轴的偏移量，`align`表示对齐方式，可选值`left`，`center`，`right`。下面 4 个方法均返回布尔值，表示是否超出页面边界。

* `upside(reference, offsetX = 0, offsetY = 0, align = 'center')` 将指定元素定位到参考元素上方。
* `downside(reference, offsetX = 0, offsetY = 0, align = 'center')` 将指定元素定位到参考元素下方。
* `leftside(reference, offsetX = 0, offsetY = 0)` 将指定元素定位到参考元素左边。
* `rightside(reference, offsetX = 0, offsetY = 0)` 将指定元素定位到参考元素右边。


### $root 对象静态方法

* `$root.scrollTop(value)` 获取或设置滚动条当前的纵向位置。
* `$root.scrollLeft(value)` 获取或设置滚动条当前的横向位置。
* `$root.visibleWidth()` 获取当前可视范围的宽度，不建议使用。
* `$root.visibleHeight()` 获取当前可视范围的高度, 在iframe中时不准，不在iframe时也不准，不建议使用。
* `$root.documentWidth()` 获取文档的宽度。
* `$root.documentHeight()` 获取文档的高度。

## Ajax 相关方法

Ajax 用来从接口调取数据，以下 4 个方法分别对应 4 种不同的 Method，不支持跨域。

* `$GET(url, path = '/')`
* `$POST(url, params = '', path = '/')`
* `$PUT(url, params = '', path = '/')`
* `$DELETE(url, path = '/')`

这 4 个方法中，`url`表示要请示的地址；`params`表示要发送的数据或参数，格式与地址字符串相同；`url`和`params`均支持 [Express 字符串](/root.js/express.md)；`path`表示获取数据成功后解析数据的 JsonPath。

* `sync(enabled)` 设置请求是同步还是异步请求，默认为异步，只有需要设置为同步时才需要使用这个方法。
* `send(func)` 设置请求发起时的回调方法。
* `complete(func)` 设置请求完成时调用的回调方法。
* `error(func)` 设置请求失败时调用的回调方法。
* `success(func)` 设置请求成功时调用的回调方法。

完整的示例和函数参数如下：

```javascript
    $POST('/api/user', 'name=Tom&age=18', '/data')
        .send(function(url, params) {
            //url 和 params 是解析后的值，一般用于调试
        })
        .error(function(status, statusText) {
            //status 是状态码, statusText 是状态文本
        })
        .complete(function() {
            //无论成功还是失败，都执行这个函数
        })
        .send(function(data) {
            //data 表示请求成功后获得的数据
        });


```

现在已经有了更强大的请求方法：

* `$cogo(todo, element)` 可以运行 PQL 语句或请示接口（支持跨域），但需要后端配合。
    + `todo`表示要运行的 PQL 语句或要请求的接口，支持 [Express 字符串](/root.js/express.md)。当`todo`是接口时，格式为`method:url -> path`，其中`method`如果是`GET`可以省略，`path`如果是根目录`/`可以省略。
    + `element`表示发起请示的元素或对象。
    + `$cogo`返回一个 **Promise** 对象。

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

## Json 对象

root.js 提供了 Json 类和一些可用的方法，以方便 Json 数据类型的解析。

Json 构造函数和方法有：

* `Json(strOrObject)` 构造函数参数可传入字符串或对象，并尝试转成 Json 对象。
* `find(path)` 根据 JsonPath 查找数据。
* `toString()` 将 Json 对象转成字符串。

静态方法有：

* `Json.parse(json)` 返回一个 Json 对象，功能同构造函数。
* `Json.toString(v)` 尝试将一个 Json 对象转成字符串。

## String 字符串类型扩展方法

* `$includes(str, delimiter = ',')`  判断字符串是否包含指定字符，类似于 SQL 中的`in`，如字符串`'1,3,14,15,28'`中是否包含`'4'`，返回`false`，是否包含`14`，返回`true`
* `$length(min = 0)` 可以把中文字符识别为`2`个位置，`'中文'.$length()`结果为`4`
* `$p(element)` [Express 字符串](/root.js/express.md)解析方法，可传入当前元素或对象。
* `$remove(str, delimiter = ',')`  如字符串 `'1,3,14,15,28'` 中移除`14`，返回`'1,3,15,28'`
* `$replace(sub, rep)` 用`req`替换字符串中的全部特定内容`sub`
* `$trim(char1, char2)` 移除字符串两端的指定字符，如移除左右括号 `str.$trim('(', ')')`
* `$union(strOrArray, delimiter)` 用指定内容拼接字符串或数组，返回值为拼接好的字符串
* `eval()` 对字母串求值，功能同全局函数`eval(str)`。
* `encode()` 进行 HTML 编码。
* `decode()` 进行 HTML 解码。
* `drop(length)` 从字符串左边开始删除指定长度的内容，如`'abcd'.drop(1) = 'bcd'`。
* `dropRight(length)` 从字符串右边开始删除指定长度的内容，如`'abcd'.dropRight(1) = 'abc'`。
* `take(length)` 从原字符串开头开始截取指定长度并返回，如`'abcd'.take(3) = 'abc'`。
* `takeRight(length)` 获取原字符串从尾部开始到指定长度的内容，如`'abcd'.takeRight(3) = 'bcd'`。
* `takeAfter(value)` 从字符串中获取第一次出现`value`之后的内容，如`'abbcd'.takeAfter(b) = 'bcd'`。
* `takeAfterLast(value)` 从字符串中获取之后一次出现`value`之后的内容，如`'abbcd'.takeAfter(b) = 'cd'`。
* `takeBefore(value)` 从字符串中获取第一次出现`value`之前的内容，如`'abcd'.takeBefore(c) = 'ab'`。
* `takeBeforeLast(value)` 从字符串中获取最后一次出现`value`之前的内容，如`'abcddcba'.takeBeforeLast('c') = 'abcdd'`。

一些字符串转化方法：

* `recognize()` 自动识别字符串的类型并转换，可识别布尔值、整数和小数，如`'yes'.recognize()`返回`true`，`'23'.recognize()`返回`23`。
* `toInt(defaultValue = 0)`  将字符串转化为整数，比`parseInt`方法更智能
* `toFloat(defaultValue = 0)`  将字符串转化为小数，比`parseFloat`方法更智能
* `toBoolean(defaultValue = false)` 将字符串转化为布尔值，会把`yes`, `1`, `true`, `ok`, `on`识别为`true`，会把`no`, `0`, `false`, `cancel`, `off`识别为`false`
* `toArray(delimiter = ',')` 将字符串转化转化数组，类似于`split`方法，也可以把`'[1,2,3,4]'`解析成数组
* `toMap(delimiter = '&', separator = '=')` 将字会串转化为 Object 结构，比较常用的是地址参数
* `toJson()` 将字符串转成Json对象
* `toCamel()` 将分隔符形式的字符串转驼峰格式，如`'mine-type'.toCamel()`结果是`mineType`
* `toPascal()` 将分隔符形式的字符串转 Pascal 格式，如`'mine-type'.toCamel()`结果是`MineType`
* `toHyphen()` 将驼峰格式的字符串转分隔符格式，如`'mineType'.toHyphen()`结果是`mine-type`

一些字符串判断方法：

* `isEmpty()` 判断字符串是否为空。
* `ifEmpty(other)` 若原字符串为空返回 other ，若部位空则返回原字符串。
* `isObjectString()` 判断字符串是否是可转为对象。
* `isArrayString()` 判断字符串是否可转为数组。
* `isIntegerString()` 判断字符串是否可转为 Int 类型。
* `isNumberString()` 判断字符串是否可转为 Number 类型。
* `isDateTimeString()` 判断字符串是否为日期时间。
* `isDateString()` 判断字符串是否为日期。
* `isTimeString()` 判断字符串是否为时间。


## Number 数字类型扩展方法

* `kilo()` 为数字增加千分符，返回字符串。如`12004.kilo()`结果是`'12,004'`
* `toPercent(digits = 2)` 将数字转换为百分数字符串，默认保留两位小数。如`0.65487.toPercent()`结果是`65.49%`
* `toTimeSpan(max)` 将秒单位的数字转成易读的时间间隔值，如`1d24h`。

## Array 数组类型扩展方法

* `$asc(p)` 升序排序原数组。如果是对象数组，可按照对象名称为`p`的属性值进行排序。
* `$desc(p)` 降序排序原数组。如果是对象数组，可按照计算对象名称为`p`的属性值进行排序。
* `distinct()` 将数组的元素进行排重，还是返回数组。
* `intersect(array)` 只保留两个数组都有的内容，即取交集。
* `max(p)` 获取数组中的最大值。如果是对象数组，可计算对象名称为`p`的属性取最大值。
* `min(p)` 获取数组中的最小值，如果是对象数组，可计算对象名称为`p`的属性取最小值。
* `minus(array)` 从原数组中删除`array`中包含的所有内容，即取差集。
* `repeat(t)` 将`t`个原数组拼接，即重复`t`次。
* `sum(p)` 获取数组各项累加的结果。如果是对象数组，可计算对象名称为`p`的属性取合计值。
* `toSet()` 将数组转化为 Set 对象，元素会进行排重。
* `union(array)` 连接另外一个数组并排重，即取合集，仅连接不排重可使用`concat(array)`方法。

## 其他全局方法

页面加载相关：

* `$ready(func)` 当页面渲染完成之后执行指定的函数，在`window.onload`事件之前，可使用多次。
* `$finish(func)` 当 Model 加载数据完成之后执行指定的函数，可使用多次。

解析地址字符串：

* `$query(name)` 获取地址中名称为`name`的参数。
* `$query.contains(name)` 判断地址中是否包含名称为`name`的参数。

创建元素：

* `$create(tag, properties, styles, attributes)` 创建一个标签元素，返回创建的元素。
    + `tag` 标签名，如`DIV`
    + `properties` 属性列表，必须是一个 Object，如`{ "className": "banner", "innerHTML": "Hello World" }`
    + `styles` 样式列表，必须是一个 Object，如`{ "backgroundColor": "#FF0000", "font-size": "2rem" }`
    + `attributes` 自定义属性列表，必须是一个 Object，如`{ "sign": "TEST" }`

全局转化方法，可处理`null`值，如果传入值为`null`，则返回默认值

* `$parseString(value, defaultValue = '')` 将数值转成字符串
* `$parseInt(value, defaultValue = 0)`  将数值转成整数
* `$parseFloat(value, defaultValue = 0)` 将数值转成小数
* `$parseBoolean(value, defaultValue = false)` 将数值转成布尔值
* `$parseArray(value, defaultValue = [])` 将数值转成数组

其他更多

* `$lang()` 获取当前浏览器的语言。
* `$size(o)` 尝试得到一个对象的长度，如字符串、数组、Object 等。
* `$random(begin, end)` 随机取`begin`到`end`之间的整数。
* `$randomX(digit = 10)` 随机取`digit`位数字字符串
* `$randomPassword(digit = 7)` 随机生成`digit`位密码，包含大小写字母和数字。
* `$guid()` 随机生成`guid`，由当前时间和`10`位随机数字和字母构成。

## 与组件编程相关内容

root.js 还包含其他与组件相关的操作，以方便开发组件。

### 组件初始化

root.js 提供了快速定义类属性的方法，这对复杂类非常好用，可简化属性定义的工作，减少大量的判断。

```javascript
class ExampleClass {    
    constructor(elementOrObject) {
        $initialize(this)
            .with(elementOrObject)
            .burnAfterReading() //阅后即燃
            .declare({
                name: 'Example_1';
                school: '',
                age: 18,
                married: false,
                grade: function(e) {
                    return e == null ? 'A' : e.toUpperCase();
                },
                gender: 'male|female',
                course: ['Math', 'English'],
                score: Enum('A', 'B', 'C', 'D'),
                plan: null,
                tagClass: 'css',
                container: '$s',
                awards: '$x',
                tag: ['Young', 'Strong'],
                onstudy：function(time) { }

            })
            .elementify(element => {
                //如果 elementOrObject 是元素
            })
            .objectify(object => {
                //如果 elementOrObject 是对象
            });
    }
}
```

上例中，`$initialize`方法传入当前类，`with`方法传入构造函数的参数，`declare`方法用于定义类属性，`elementify`方法用来定义如果是元素该执行什么操作，`objectify`方法用来定义如果是对象该执行什么操作，`burnAfterReading`表示初始化完成后删除所有属性定义（一般用不到）。特别说明一下`declare`方法，这个方法接受一个 Json 对象，会根据参数的值来判断数据类型并且当未设定属性时，就使用给定的默认值。这里支持非常多的类属性，分别说明如下：

* `name`属性，表示名称或 ID，系统会自动查找元素或对象的`id`或`name`，如果实在找不到时再使用默认值。
* 字符串，可设置为非保留字字符串为默认值，如上例中的`school`属性默认值为空字符串。
* 数字，可设置任意数字为默认值，如上例中的`age`属性。如果默认值为小数，则类型为小数；否则为整数。
* 布尔值，可设置`true`或`false`为默认值，如上例的`married`属性。
* 空值，当未设定属性时，即保留属性值为`null`，如上例的`plan`。
* 函数，如果要对给定的属性值进行再加工或不能使用基本数据类型进行判断，可使用函数类型，初始化时会按照给定函数逻辑进行运算，如上例的`grade`属性。
* 事件，事件必须以`on`开始，一般`on`后面跟一个动词，默认值是一个空函数，函数可以有参数。
* 枚举，枚举有两种声明方式可选，如上例`gender`和`score`属性，设置值必须是在枚举中的全大写形式的字符串，如果不在枚举中，将自动使用第一个枚举值。注意在使用时必须用大写形式。
    ```javascript
    if (student.gender == 'MALE') {
        ...
    }
    ```
* 子标签属性，如`tipTitle: 'tip[Title]'`，即`tipTitle`属性的默认值为其子标签 TIP 属性`title`的值。
* `html`属性，除了可以在元素属性上定义之外，还可以定义为元素的子标签，例如 TreeNode 类的 text 属性。
    ```html
    <treenode text="Hello World"></treenode>
    <treenode>
        <text>Hello World</text>
    </treenode>
    ```
    `declare`程序会自动查找`text`属性或 TEXT 标签的值。
* `css` CSS 样式，一般情况下可使用字符串，当需要从子标签获取样式属性时，可使用默认值`css`。如上例`titleClass`。
    ```html
    <tag class="bold"></tag>
    ```
* `$x` 元素或元素集合属性，类型为`$root`，一般用于元素操作。赋值时一般为字符串（CSS选择器），使用时会自动转化类型。如上例`awards`属性。
* `$s` 单个原生元素属性，可用于元素操作。赋值时一般为字符串（CSS选择器），使用时会自动转化类型。如上例的`container`属性。

除上面说明的属性值类型之外，还需要说明一下获取属性的逻辑。

* 属性声明时，如果属性名有多个单词，驼峰格式和分隔符格式效果相同，例如`tagClass`和`tag-class`是同一个属性，建议使用第二种形式。
* 优先获取显式定义的属性，再从元素标签的原生属性获取。
    ```html
    <span title="HELLO WORLD"></span>
    <span></span>
    ```
    第一个 SPAN 已经声明了`title`属性，那么属性值就是`HELLO WORLD`；第二个 SPAN 标签没有声明`title`属性，因为`title`是原生属性，所有属性值可以得到空字符串而不是`null`。


### 为组件定义事件

属性、方法和事件是组件的三个要素，“事件”是组件与用户交互的基础。root.js 为自定义标签提供了监听和执行事件的方法。

上一节提到了可以使用`declare`方法定义事件，还可以使用原型方法为为自定义标签添加事件。

```javascript
Tag1.prototype.onclick = function(p) { };
```

事件本身就是一个方法，但需要在逻辑中适当的位置自动执行这个方法。`declare`方法会自动为自定义组件添加添加`execute`方法，用于在编写组件时调用事件：

```javascript
this.execute('onclick', arg1, arg2);
```

或者也可以使用全局方法执行事件。

```javascript
Event.execute('#Tag1', 'onclick', arg1, arg2);
```

使用时可以有多种方法为自定义组件添加事件。

1. `declare`方法自动为自定义组件添加`on`方法，用于为组件添加监听事件。这个方法必须等待组件加载完成。一般在`$finish`全局方法里使用。`on`方法可以添加多个事件函数，按照添加顺序执行。

```javascript
$t('#Tag1').on('click', function(p) {
    //...
});
```

2. 使用全局方法为自定义组件添加事件。这个方法可以不等待组件加载完成即可使用。

```javascript
$listen('#Tag1').on('click', function(p) {
    //...
})
```

3. 直接为组件的事件赋值。这个和`on`方法一样，必须在组件加载完成之后才能使用。不同的是这个只能添加一个事件函数，而`on`可以添加多个事件函数。这种方式和`on`方法添加的事件函数不冲突，调用事件时会依次执行这些事件函数。这种方式添加的事件函数优先级高于`on`方法添加的事件函数。

```javascript
$t('#Tag1').onclick = function(p) {
    //...
}
```


## 自动执行的功能

除上面介绍的所有开发者可用的方法外，还有一些自动执行的功能。

* 含有`enter`属性的 INPUT 标签会自动添加回车事件，当用户按下回车时，如果文本框的值改变，得自动跳转到`enter`属性指定的地址。
    ```html
    <input type="search" enter="/search?key=$:[value]">
    ```

* 自动解析标签的特定属性值，如果这些属性值里包含 [Express 字符串]的占位符，将自动替换成对应的值。这些属性包括：
    + `-html` 任意标签
    + `-value` INPUT 标签的值
    + `-title` 任意标签的`title`属性
    + `-href` A 标签的链接属性
    
    ```html
    <span -html>ID: &(id)</span>
    <input type="text" -value="&(name)">
    <div -title="Hello $(#Name)">...</div>
    <a -href="/detail?id=&(id)">Detail Page</a>
    ```
    
    这几个以中线开头的属性会在页面加载时替换成解析后的值。

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