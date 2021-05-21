# Express 字符串

在日常开发中，经常需要拼接各种各样的字符串，这些字符串的值由页面内容（比如标签（元素）的内容、地址参数等）组成。**Express 字符串** 提供了使用占位符的方法来组成字符串，这是一种非常简单的方法。使用 Express 方法，字符串不需要拼接，只需要简单的设置占位符即可。Express 字符串支持两种格式的占位符，分别是 URL 地址参数占位符和 DOM 操作符。

## Express 字符串应用范围

Express 字符串应用范围比较多

* 任意元素的 innerHTML 中，需要在元素中添加`-html`属性，可以为任意值
* A 元素的`href`属性，需要在`href`属性名前加中横线 `-href`
* INPUT 元素的`value`属性，同样也要在前面加中横线 `-value`
* 任意元素的`title`属性，同样也要在属性前面加中横线`-title`
* FOR 标签的`in`属性
* 与数据加载有关的标签的`data`属性，如 TEMPLATE、TABLE 等
* 一些组件的扩展属性，如`value:`
* 其他在组件中提到的属性，如`check`等

另外，root.js 中与 Ajax 相关的几个方法的 URL 地址或能写 PQL 语句的地方都支持使用 Express 字符串。Express 字符串会逐渐增多。

## URL 地址参数占位符

通过地址参数占位符可以直接获取 URL 地址中传递的参数。占位符格式 `&(param)`。地址参数占位符不支持`defaultValue`格式，如果参数名称不存在，则值为`null`。下面会介绍用法。

```html
<a -href="/item/detail?id=&(id)">
```

如果使用了 [Voyager 模板引擎](/voyager/overview.md)，则以上代码与以下代码功能完全相同。

```html
<a href="/item/detail?id=#{id}">
```

区别是前者在客户端浏览器中进行计算，后者在服务器端进行计算。

## DOM 操作符

在前端功能开发中，经常需要用选择器获取标签，读取标签的值或属性，组件库提供了更方便快捷的方法进行 DOM 导航。

DOM 占位符标准格式为`$(selector)+-><n[attr]?(defaultValue)`，各个符号的含义分别说明如下：

* `$` 必须使用
* `(selector)` 选择器，圆括号不能省略，选择第一个满足条件且可见（`hidden`为`false`）的元素，可以选择扩展元素，功能同`$v`。例如`$(#Input1)`、`$(#Input1,#Input2)`。
* `$:` 表示当前元素或对象
* `+`或字母`n` 导航符号，代表导航到下一个兄弟节点
* `-`或字母`p` 导航符号，代表导航到上一个兄弟节点
* `<`或字母`b` 导航符号，代表导航到父级节点
* `>`或字母`f` 导航符号，代表导航到第一个子节点
* 数字`0`到`9` 代表导航到第`n`个子节点
* 字母`l` 代表最后一个子节点
* `[attr]` 代表元素属性，方括号不能省略。如果不使用属性，能获取`value`或`innerHTML`
* `?(defaultValue)`  当返回值为`null`时，设置默认值
* 当遇到字符冲突时，可使用`!`结尾，例如`<span -html>$(h1)!</span>`，表示占位符符结尾。如果这种情况下还要输出叹号`!`，那么输入两个叹号`!`即可。

可以多个不同或相同的导航符号进行组合，以定位到需要的元素。几个示例：

* 调取 name 为`keyword`的[扩展文本框](/root.js/input.md)加值 `$(#keyword)`
* 调取 id 为`title`的节点的下一个元素的 textContent `$(#title)+`
* 调取 id 为`link`的节点的`target`属性的值  `$(#link)[target]`
* 调取当前元素的`sign`属性 `$:[sign]`
    如 `<a sign="link" -href="/hello/$:[sign].htm">`
    解析结果为 `<a sign="link" href="/hello/link.htm">`

在 Express 字符串中，表示上一个同胞节点的第一个子节点的符号 `->` 在 root.js 中具有特殊的意义，尽量避免使用这个组合，可使用`-f`、`pf`、`p>`代替。

## Javascript 短句和语句

当上面介绍的操作不能满足数值加工需求时，可使用 **Javascript 短句** 做值做进一步处理。Javascript 短句占位符格式为 `~{ javascript expression... }`。一般情况下，其中的`~`可以省略。

短句类似于赋值语句的右半部分，如：

```html
<input type="text" update-on="click:#Button" value:="~{ this.value.toHypen() }" />
```

极端情况下，业务逻辑有可能更复杂，短句也不能满足需求了，还可以使用 **Javascript 语句占位符**，格式为 `~{{ javascript statement... }}`。语句类似于一个函数内的所有语句，可以对数据进行任何操作，但必须通过`return`返回值。一般情况下，其中的`~`可以省略。如下例：

```html
    <a href="/page/~{{
            if ($(age) > 18) {
                return 'adult';
            }
            else if ($(age) < 12) {
                return 'children';
            }
            else {
                return 'youth';
            }
        }}.html">Detail</a>
```

## 在 Javascript 语句中使用地址参数和 DOM 占位符

可以使用字符串的扩展方法`$p`来解析含有占位符的字符串

```javascript
let str = '&(param)-$(#title)'.$p();
```

如果要传递从某个元素开始，可以把元素当参数传递进去

```javascript
let str = '&(param)-$:[title]'.$p(span);
```

## Javascript 表达式中的默认变量

在 Javascript 两种占位符中，可以使用三个预设的变量，分别为`value`、`text`和`data`，指定当前组件的属性或回调函数的变量。分别解释如下：

* `value` 指向当前组件的`value`属性值，如 Input、Select等组件都有`value`属性。
    ```html
    <input id="Username" onchange+="/api/check-username?name={value}">
    ```
* `text` 指向当前组件的`text`属性值，如 Select 组件都`text`属性（表示当前选中项的文本）。
    ```html
    <select onchange+="/api/options?title={text}&id={value}">
    ```
* `data` 指向请求接口或执行 PQL 语句返回的结果。下例中当执行失败时显示返回的数据信息。
    ```html
    <button id="TestButton" onchange+="/api/system/test-connection" expection="empty" failure-text="连接失败：{data}">Test<button>
    ```

以上三个变量都可以继续操作，如`{ data.message }`，`{ value.trim() }`。这三个变量也可以用到 Javacript 语句占位符中`{{ return data.status + value; }}`。

## 对结果值进行 URI 编码

特别是在接口地址中，经常要传递各种数据，就避免不了要对传递的值进行编码，即 Javascript 的 `encodeURIComponent` 全局方法。可以使用百分号`%`作为占位符的结尾，以表示对结果值进行 URL 编码。因为程序并不清楚什么时候需要编码，什么时候不需要编码，所以编码操作需要开发者主动进行。编码符号`%`适用于以上介绍的所有占位符。

```html
<table data="/api/search?keyword=$(#Keyword)%"></table>
```

很少的情况下会遇到字符冲突，例如：

```html
<table data="SELECT * FROM qross_api_in_one WHERE path LIKE '$(#Filter)%'"></table>
```

这时的本意是匹配以关键词开始的所有结果，`%`在这里有特殊的意义。解决方法是使用 Javascript 表达式，稍微得多写几个字符。

```html
<table data="SELECT * FROM qross_api_in_one WHERE path LIKE '$(#Filter)~{'%'}'"></table>
```

其实写两个也可以，`path LIKE '$(#Filter)%%'`。

---
参考链接

* [data 扩展属性](/root.js/data.md)
* [Model 数据加载模型](/root.js/model.md)