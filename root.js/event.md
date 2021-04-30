# 精简事件和表达式

为了简化页面制作，进一步消除页面上的 Javascript 代码，root.js 提供了精简事件和事件表达式，还可以在目标上绑定事件。

## 精简事件

```html
<span id="Span1" data="...">...</span>
<input id="TextBox1" />
<button id="Button1" onclick-="reload: #Span1">Button</button>
<select id="Select1" onchange-="update-value: #TextBox1 -> $(#Select1)">
```

上例中的`onclick-`和`onchange-`即为精简事件，其中的减号`-`表示“精简”的意思，以和原生的事件名称区分开来。而且，精简事件只能以属性形式写在标签上。精简事件支持所有客户端事件。

## 事件表达式

精简事件的值即为事件表达式。上例中`reload: #Span1`和`update-value: #TextBox1 -> $(#Select1)`，与之对应的 Javascript 代码为 `onclick="$s('#Span1').reload()"`和`onchnage="$x('#TextBox1').update('value', '$(#Select1)')"`。

事件表达式格式为`method: selector -> value`，共分三部分：方法名、CSS 选择器和方法参数，其中`method`和`selector`之间使用冒号分隔`:`，`selector`和`value`之间使用箭头号分隔`->`。其中`selector`和`value`可以省略，省略`selector`表示调用对象本身的方法，省略`value`表示调用无参方法。特别说明：方法名各单词之间使用`-`分隔，最后一个`-`之后的值为方法的第一个参数。多个`selector`和`value`之间使用逗号分开，`method`不支持设置多个。多个表达式之间使用分号`;`分隔。`value`参数支持 [Express 表达式](/root.js/express.md)。

以下均为合法的表达式格式。

```html
<a onclick-="method">
<input onblur-="method: selector; method: selector1, selector2;">
<button onclick-="method: selector -> value; method: selector -> value1, value2;">
<select onchange-="method: selector1, selector2 -> value1, value2, value3;">
```

事件表达式的应用暂时比较少，会慢慢扩展到更多标签上。

```html
<select id="Select1" data="..." onchange-="relaod: #Select2">
<select id="Select2" data="..." onchange-="reload: #Select3">
<select id="Select3" data="...">
```

上例实现了一个 SELECT 扩展标签的三级联动。

## 反向事件绑定

另一种方式是在目标标签上绑定事件监听，有时逻辑更加清晰。例如上面 SELECT 三级例子，可以这样写：

```html
<select id="Select1" data="...">
<select id="Select2" data="..." reload-on="change: #Select1">
<select id="Select3" data="..." reload-on="change: #Select2">
```

即属性名为方法名称加上`-on`后缀，属性中的表达式方法的位置换成事件名称，可以省略`on`前缀。后面部分与正向的事件表达式相同。每个扩展标签只有少量方法支持这么做，会在相应的标签文档中说明。事件名只支持一个，但是选择器支持多个，使用逗号分隔。更多示例：

* `onchange:#SelectButton1,#Select2` 事件名的`on`可以省略。当`SelectButton1`和`Select1`选择项改变时触发。
* `focus:input[type=text]` 当页面上所有 INPUT 输入框的获得焦点时执行。

## 服务器端事件的状态事件

在[服务器端事件](/root.js/server.md)中，执行完成后会触发几个客户端事件，例如`onclick+`对应的是`onclick+success`、`onclick+failure`、`onclick+exception`和`onclick+completion`。这几个事件也支持精简格式，例如：

```html
<button onclick+="update ...." onclick+exception-="html: #ErrorSpan -> {data}">Update Button</button>
<span data="..." reload-on="click+success:#Button1">...</span>
<input update-value-on="change+success: #Select1 -> $(#Select1)" />

<button id="Button1" onclick+="insert into ...">Button</button>
<select id="Select1" onchange+="/api/select/options">...</select>
```


---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [与数据相关的扩展属性](/root.js/data.md)
+ [服务器端事件](/root.js/server.md)
* [SELECT 选择器扩展](/root.js/select.md)
