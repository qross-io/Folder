# Button 按钮扩展

标签库对 BUTTON 标签进行了扩展，主要是为了实现与后端进行快速交互。当设置了`onclick+`或`watch`属性时，所有扩展功能才生效。

```html
<button id="Button1" onclick+="pql|api" watch="#TextBox1,#TextBox2" scale="normal" color="blue" confirm-text="" click-text="" success-text="" failure-text="" exception-text="" jumping-text="" jump-to="url" enabled-class="" disabled-class="">按钮</button>
```

## 使用

需要引入两个文件，注意`root.button.js`文件的引入要在其他表单组件如`root.input.js`之后。

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.popup.js"></script>
<script type="text/javascript" src="/root.button.js"></script>
```

<script type="text/javascript" src="@/root.input.js"></script>
<script type="text/javascript" src="@/root.button.js"></script>

## 扩展属性

与按钮操作相关的扩展属性有：

* `disable-on-click` 当向服务器端发送请求时，是否禁用按钮，可以防止用户多次点击，默认为`true`。
* `enable-on-success` 当服务器端请求成功后，是否启用按钮，默认为`true`。如果为一次性按钮可设置为`false`。
* `enable-on-failure` 当服务器端请求结果不符合预期时，是否启用按钮，默认为`true`。如果为一次性按钮可设置为`false`。
* `enable-on-exception` 当服务器端请求出错时，是否启用按钮，默认为`true`。
* `href` 请求成功后的跳转 URL 地址，支持 [Express 字符串](/root.js/express.md)。没有服务器端事件时，可以把按钮直接当链接使用。
    ```html
    <button href="/index.html">返回首页</button>
    ```
* `text` 获取或设置按钮的文字。
* `type` 按钮类型，目前支持`switch`和`twice`属性，用作切换按钮和再次确认按钮。本文最下面有示例。
* `watch` 用于组件监听，下面在“组件监听”中单独介绍这个属性。

与启用禁用相关的属性有：

* `disabled` 是否默认禁用按钮，原生属性，不过属性值支持比原生多，接受可识别为尔值的各种值，如`ok`、`cancel`、`false`、`0`等。
* `disabled-class` 按钮在禁用状态下的样式，默认值`normal-button optional-button`。
* `disabled-text` 按钮在禁用状态下的文本，默认值`Disabled`。
* `disabled-value` 按钮在禁用状态下的值，默认值`no`。
* `enabled` 是否默认启用按钮，与`disabled`属性相对，两个属性同时使用时优先级低于`disabled`。接受可识别为布尔值的值，如`yes`、`no`、`true`、`1`等。
* `enabled-class` 启用状态下的样式，默认值`normal-button blue-button`。
* `enabled-text` 按钮在启用状态下的文本，默认值`Enabled`。
* `enabled-value` 按钮在启用状态下的值，默认值`yes`。

与提示相关的属性如下，所有以`-text`消息相关属性均支持 [Express 字符串](/root.js/express.md)：

* `hint`或`hint-element` 设置在其他文本标签中提示操作过程中的各种信息，属性值为 CSS 选择器，如`#Message`。当所有提示方式都没有设置但是又设置了提示文字属性时，默认在按钮右侧创建一个 SPAN 标签用于显示提示文字。
* `callout`或`callout-position` 使用 Callout 提示，属性值用来设置显示的位置，默认在按钮上方显示即`up`。
* `alert` 使用 Alert 对话框进行提示，无属性值。如果引入了 Popup 组件，则会代替原生的 window.alert 对话框。
* `message`或`message-duration` 使用 Message 组件进行提示，属性值为 Message 提示框显示的时间，`0`为一直显示，单位为秒。
* `text-class` 提醒文字的样式，仅适用于 Hint 提示方式。
* `error-text-class` 错误或异常文字的样式，仅适用于 Hint 提示方式。
* `valid-txt-class` 正确或成功文字的样式，仅用于 Hint 提示方式。
* `confirm-text` 如果设置了这个属性，则在点击按钮时显示确认提示框，提示框里显示这个属性的文字。
* `confirm-title` 设置确认对话框的标题文字，对于原生确认对话框无效。
* `confirm-button-text` 设置弹出对话框确认按钮的文字，默认为`OK`，对于原生确认对话框无效。
* `cancel-button-text` 设置弹出对话框取消按钮的文字，默认为`Cancel`，对于原生确认对话框无效。
* `click-text` 点击按钮触发服务器端请求时的提示文字，替换按钮上的文字。
* `invalid-text` 当 onclick 事件的值返回 false 时的提醒文字`onclick="return false;"`。
* `jumping-text` 跳转过程中的提示文字，替换按钮文字。
* `success-text` 请求成功后的提示文字，本文下方有状态说明。
* `failure-text` 请求失败后的提示文字，本文下方有状态说明。
* `exception-text` 请求发生异常后的提示文字，替换按钮上的文字，“异常”是指接口或 PQL 语句发生错误，错误会被打印到控制台。


与按钮外观相关的属性有：

* `color` 按钮颜色，见本文下面的说明。
* `scale` 按钮大小，见本文下面的说明。

## 扩展方法

扩展方法可用于[事件表达式](/root.js/event.md)。

* `enabld()` 启用按钮。
* `disable()` 禁用按钮。

还有一个通用的`set(attr, value)`扩展方法可以用，原生方法不影响使用，如`click()`方法。

## 扩展事件

按钮新增一个[服务器端事件](/root.js/server.md)`onclick+`，是按钮扩展的关键属性。属性值为按钮要执行的 PQL 语句或要请求的 API 接口，属性值格式详见[服务器端事件](/root.js/server.md)和[与数据相关的属性](/root.js/data.md)。客户端事件`onclick`依然可以使用。

为了配合`switch`按钮类型，新增两个客户端事件`onclick-enabled`和`onclick-disabled`，分别当按钮切换到相应的状态下触发。  
为了配合`twice`按钮类型，新增两个客户羰事件`onclick-confirm`和`onclick-cancel`，分别当按钮确认和取消按钮时触发。

客户端事件可配合其他组件使用，比如需要监听按钮的动作，参见[精简事件和表达式](/root.js/event.md)。

## 可用的选择器

扩展选择器一般用来定义事件，可用的选择器只有 1 个：`$('#id')`或`$s('#id')`， 其中`id`前面的`#`符号不可省略。一般情况下用不到选择器，只了解选择器的语法即可。

## 请求成功和失败

Button 扩展通常用于通过`onclick+`事件向后端发起请求，请求有以下几种状态：

* `success` 成功状态，表示返回值符合预期，依照返回值类型确定。详见[服务器端事件](/root.js/server.md)中的说明。
* `failure` 失败状态，表示返回值不符合预期，依照返回值类型确定。    
* `exception` 请求异常状态，PQL 语句或接口出错。

大多数情况下，按钮行为都是向数据库插入或更新数据，插入数据可以返回最新数据的自增`id`，更新数据可以返回受影响的行数，使用`non-zero`规则即可进行判断。

## 组件监听

Button 一般情况下会配合其他表单组件使用，如文本框等。Button 提供了对其他表单组件进行监听的功能，已在表单输入未完成和完成时作出正确的响应，决定是否禁用按钮。这个响应是自动的，一般情况下也不需要专门配置。

```html
<button id="Button1" onclick+="insert into table1 (title, views) values ('$(#Title)', $(#Views))">Add</button>
<button id="Button2" watch="#Title,#Views">Add</button>
<button id="Button3" onclick+="insert into table1 (title, views) values ('$(#Title)', $(#Views))" watch="#Source">Add</button>
```

监听其他组件有两种方式。第一是设定`onclick+`事件，页面加载时会自动分析`onclick+`事件中相关的组件是否为必填项`required`，Button 会自动将必填组件加入监听列表。第二是设定`watch`属性，Button 会自动将`watch`属性中的组件加入监听列表，只能按照组件的名字或 id 进行监听，多个组件之间使用逗号`,`分隔。当只使用一种方式不能满足监听要求时，也可以两种方式混合使用。上例中，假如`#Title`，`#Views`，`#Source`都是必填项，则按钮必须在这 3 个表单项的输入值都正确时才会启用。

试试下面的例子，文本框输入值时 Button 才能启用。

<input id="Input1" type="text" required-text="Please input something." invalid-text="Please input 3 characters at least." minlength="3" size="30" />

<button watch="#Input1"> Submit </button>

示例代码如下：

```html
<input id="Input1" type="text" required-text="Please input something." invalid-text="Please input 3 characters at least." minlength="3" size="30" />
<button watch="#Input1"> Submit </button>
```

## 可用的按钮外观

按钮样式分为基本样式和颜色样式，基本样式用于控制按钮或文字大小，颜色样式用于控制按钮颜色，一般情况下两个样式同时使用。

* `large-button` 超大按钮，文字大小`1.25rem`或`20px`
* `big-button` 大按钮，文字大小`1.125rem`或`18px`
* `medium-button` 中等按钮，文字大小`1rem`或`16px`
* `normal-button` 标准按钮，文字大小`0.875rem`或`14px`
* `small-button` 小按钮，文字大小`0.75rem`或`12px`
* `mini-button` 超小按钮，文字大小`0.625rem`或`10px`

颜色样式有：

* `prime-button` 系统主题颜色相同，`32`种颜色中随机一种
* `green-button` 绿色按钮
* `red-button` 红色按钮
* `optional-button` 无色按钮，与其他颜色按钮相反
* `maroon-button` 深红色按钮
* `blue-button` 蓝色按钮，默认颜色
* `orange-button` 橙色按钮
* `gray-button` 灰色按钮
* `white-button` 白色按钮

示例：

```html
<button class="normal-button orange-button" disabled-class="normal-button gray-button">...</button>
```

外观样式可用`scale`和`color`属性代替，如

```html
<button scale="small" color="prime">...</button>
```

## 切换按钮示例

当`type`属性设置为`switch`时，可以把按钮做为切换按钮使用。

```html
<button id="EnableButton" type="switch" enabled-text="已启用" enabled-value="yes" disabled-text="已禁用" disabled-value="no" value="<%= $job.enabled %>" onclick="return <%=$job.dags%> > 0;" invalid-text="至少先设置一个工作流命令才能启用调用。" onclick+="put:/api/job/switch?id=&(jobId)&enabled={value}" />
```

这个示例来自于 Master 项目后台启停用调度的按钮，主要逻辑为：只有先设置了工作流之后才能启用按钮，由`onchange`事件来判断，如果不满足启用条件，则提示文字`invalid-text`；`options`用来设置不同状态下按钮的显示文字和值；`value`属性必须设置值；`onclick+`事件用于请求后端接口，完成数据库更新。

## 二次确认按钮示例

当`type`设置为`twice`时，当点击按钮时会划出“确认”和“取消”按钮进行再次确认，在用户体验上这种方式比弹出框更友好一些。

```html
<button id="RestartButton" type="twice" class="task-failed-badge normal-button w150" confirm-button-text="# confirm-restart #" onclick+="put:/api/keeper/quit-on-next-beat?node_address=<%=$node.node_address%>" onclick+success-="start: #Logs; fadeIn: #LoadingHint" exception-text="Exception: {data}" callout cancel-button-text="# cancel-restart #" enable-on-success="false"># restart-button #</button>
```

这个示例来自于 Master 项目后台重启 Keeper 的按钮，主要逻辑为，当点击按钮时，按钮会向下划出（fadeOut），同时“确认”和“取消”两个按钮会从左右两方划入（fadeIn）；再次点击“确认”按钮才会继续执行。使用`confirm-button-text`和`cancel-button-text`属性可以设置“确认”和“取消”按钮的文字，`enable-on-success`属性表示点击成功后不再恢复。

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [与数据相关的属性](/root.js/data.md)
* [精简事件和表达式](/root.js/event.md)