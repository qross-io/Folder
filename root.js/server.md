# 服务器端事件

为了进一步减少 Javascript 代码的编写，root.js 为组件提供了 **服务器端事件**，服务器端事件支持向后端发送请求，例如当文本框失去焦点时、当点击按钮时。服务器端事件是前端组件与后端交互的核心属性。

```html
&lt;textarea coder="java" onsave+="/api/code/check?code={value}% # /data -> not-zero"&gt;&lt;/textarea&gt;
<button onclick+="/api/entity/update?id=&(id)">Button</button>
<input onchange+="select id from users where name='{value}' -> FIRST CELL -> IF GREATER THAN ZERO false ELSE true" />
```

* 事件名称与元素标签或组件原有的事件名称相同，只比客户端事件多一个加号`+`，注意事件名和`+`号之间不能有空格。`+`与可以理解为是客户端事件的 plus 版本，也可以理解为“扩展”、“附加”的意思。
* 服务端事件只能在元素标签上定义。
* 服务器端事件与客户端事件不冲突，原有客户端事件如`onclick`、`onblur`等依然可用。
* 只有少部分事件支持服务器端请求，以避免过多的向服务器端发送请求，会分别在相应的标签中说明。
* 服务器端事件属性格式支持 PQL 语句和接口，见[与数据相关的属性](/root.js/data.md)和 [Express 字符串](/root.js/express.md)。
* 服务器端事件可以处理返回值，下文详细说明。

## 请求接口

数据接口是最经常的与后端交互的方式。

```html
&lt;textarea coder="java" onchange+="/api/code/check?code={value}% # /data -> not-zero"&gt;&lt;/textarea&gt;
```

上例中，要请求的接口是`/api/code/check?code={value}%`；井号`#`后面的`/data`表示当接口请求完成后返回结果中路径`/data`里的数据，支持标准 JsonPath 格式；`->`后面的`not-zero`为判断接口是否请求结果是否符合预期的条件，意义为返回结果不为`0`。所有判断是否符合预期的条件和逻辑为：

+ `empty`和`non-empty`或`not-empty`：表示字符串是否为空、集合对象是否为空等，识别规则见 [$parseEmpty方法](/root.js/root.md#parse)；
+ `true`和`false`：自动将值转成布尔值，转换规则见 [$parseBoolean 方法](/root.js/root.md#parse)中的说明；
+ `zero`和`non-zero`或`not-zero`：自动将值转成`0`，转换规则见 [$parseZero 方法](/root.js/root.md#parse)中的说明；
+ 当以上可选属性值不能满足要求时，可使用 Javascript 表达式进行判断，可用变量`data`表示返回的结果，如`-> data > 10 && data < 20`或`-> data.length > 0`；
+ 如果仅仅是判断返回值是否与预期相等，可以直接写期望值，如`-> 2`。
+ 如果不设置判断条件，则会在标签设置了`success-text`或`failure-text`属性时尝试将返回结果自动转化为布尔值，转换规则同[$parseBoolean 方法](/root.js/root.md#parse)中的说明。

当符合预期时表示执行成功，提示`success-text`，当不符合预期时表示执行失败，提示`failure-text`。如果没有设置判断条件，并且没有设置`success-text`或`failure-text`属性，那么结果会永远识别为`true`。判断结果还有另一个目的，就是触发客户端事件监听，下文中单独介绍。

## 执行 PQL

PQL 是 Cogo 支持的后端交互方式，但仅支持一条 PQL 语句。

```html
<input onchange+="select count(0) from users where name='{value}' -> FIRST CELL -> IF GREATER THAN ZERO false ELSE true">
```

与接口交互方式不同，PQL 语句返回执行后的结果，不支持在前端对结果进行进一步解析和判断结果是否符合预期，所有操作都可以在 PQL 中通过 [Sharp 表达式](/pql/sharp.md)完成。上例中``FIRST CELL`用于提取 SELECT 语句返回的数据表中第一个单元格的内容，`IF GREATER THAN ZERO`当判断结果大于`0`时返回`false`。依然会将最终结果自动转成布尔值，如果最终结果可识别为`true`，则提示`success-text`，如果可识别为`false`，提示`failure-text`。如果没有设置`success-text`或`failure-text`属性，则结果总是会识别为`true`。

## 请求状态

每次服务器端请求都会有 3 种状态。

* `success` 服务器端请求成功且结果符合预期。
* `failure` 服务器端请求成功但是结果不符合预期。
* `exception` 服务器端请求失败，一般为接口或 SQL 语句错误。

每种状态都有对应状态的提醒文字和扩展事件，提醒文字属性上面已经提到，扩展事件的说明见“事件监听”。

## 提醒文字

当判断结果是否符合预期时，如果设置了提醒文字属性`success-text`或`failure-text`，则在成功或失败时提醒相应的文字，提醒文字显示位置和方式与标签或组件的设置有关，当不设置这些属性时，则不会进行提醒。当请求接口发生错误时，提醒`exception-text`，如果不设置，则提醒出错信息。这几个属性均支持 [Express 字符串](/root.js/express.md)，如：

```html
<button exception-text="Error: {data.message}">
```

## 提醒文字的显示方式

一般支持服务器端事件的组件都支持以下几种方式显示提醒文字。

* HintElement 基本都支持，如果不设置任何显示方式，则这种方式为默认。一般在组件右侧会创建一个 SPAN 标签。也可以通用`hint-element`或`hint`属性进行设置，属性值为选择器，如`hint="#Message"`。
* Callout 基本都支持，需要通过属性`callout-position`或`callout`进行设置，属性值可选`up`、`down`、`left`、`right`等，详见[基础库](/root.js/root.md)中 Callout 相关的说明。
* Message 基本都支持，需要引入动画库，也要通过属性`message-duration`或`message`设置消息框的显示持续时间，属性值为数字，单位为“秒”，设置为`0`表示一直显示。说见[动画库](/root.js/animation.md)中关于 Message 的说明。
* Alert 除非特别说明的外（如 BUTTON 按钮），一般都不支持，这种显示提示文字的方式很不友好，一般不建议使用。

## 事件监听

与服务器请求的状态对应，root.js 为所有服务器端事件提供了 3 种扩展事件和 1 个额外的事件。事件名格式为`服务器端事件名+状态`，假如服务器端事件名为`onclick+`

* `onclick+success` 当请求成功时触发。事件函数`function(data)`，参数`data`为返回的数据。
* `onclick+failure` 当请求失败时触发。事件函数`function(data)`，参数`data`为返回的数据。
* `onclick+exception` 当请求发生错误时触发。事件函数`function(error)`，参数`error`为错误信息。
* `onclick+completion` 当请求完成后触发，无论请求成功还是失败。事件函数`function()`，无参数。

与客户端事件一样，这些事件也可以被其他标签引用，详见[精简事件和表达式](/root.js/event.md)。例如：

```html
    <input update-value-on="click+completion: #Button1" />
```

root.js 建议在标签属性定义事件和事件监听。框架中还有其他的事件格式，请参阅[精简事件表达式](/root.js/event.md)及其最后一节的内容。

---
参考链接

* [与数据相关属性](/root.js/data.md)
* [Express 表达式](/root.js/express.md)
* [精简事件表达式](/root.js/event.md)