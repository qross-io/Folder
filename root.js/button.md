# Button 按钮扩展

组件库对 BUTTON 标签进行了扩展，主要是为了实现与后端进行快速交互。当设置了`onclick+`或`watch`属性时，所有扩展功能才生效。

## 使用

需要引入两个文件，注意`root.button.js`文件的引入要在其他表单组件如`root.input.js`之后。

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.popup.js"></script>
<script type="text/javascript" src="/root.dialog.js"></script>
<script type="text/javascript" src="/root.button.js"></script>
```

<script type="text/javascript" src="@/root.input.js"></script>
<script type="text/javascript" src="@/root.button.js"></script>

## 标签示例

```html
<button id="Button1" onclick+="pql|api" watch="#TextBox1,#TextBox2" scale="normal" color="blue" confirm-text="" click-text="" success-text="" failure-text="" exception-text="" jumping-text="" jump-to="url" enabled-class="" disabled-class="">按钮</button>
```

可用的扩展属性如下（按功能排序）：

* `watch` 用于组件监听，下面在“组件监听”中单独介绍这个属性。
* `confirm-text` 如果设置了这个属性，则在点击按钮时显示确认提示框，提示框里显示这个属性的文字。
* `confirm-title` 设置确认对话框的标题文字，对于原生确认对话框无效。
* `confirm-button-text` 设置弹出对话框确认按钮的文字，默认为`OK`，对于原生确认对话框无效。
* `cancel-button-text` 设置弹出对话框取消按钮的文字，默认为`Cancel`，对于原生确认对话框无效。
* `click-text` 点击按钮触发服务器端请求时的提示文字，替换按钮上的文字。
* `invalid-text` 当 onclick 事件的值返回 false 时的提醒文字 `onclick="return false;"`。
* `success-text` 请求成功后的提示文字，本文下方有状态说明。
* `failure-text` 请求失败后的提示文字，本文下方有状态说明。
* `exception-text` 请求发生异常后的提示文字，替换按钮上的文字，“异常”是指接口或 PQL 语句发生错误，错误会被打印到控制台。
* `href` 请求成功后的跳转 URL 地址，支持 [Express 字符串](/root.js/express.md)。没有服务器端事件时，可以把按钮直接当链接使用。
    ```html
    <button href="/index.html">返回首页</button>
    ```
* `jumping-text` 跳转过程中的提示文字，替换按钮文字。
* `enabled-class` 启用状态下的样式，默认值`normal-button blue-button`。
* `disabled-class` 按钮在禁用状态下的样式，默认值`normal-button optional-button`。
* `color` 按钮颜色，见本文下面的说明。
* `scale` 按钮大小，见本文下面的说明。
* `type` 按钮类型，目前仅支持`switch`属性，用作切换按钮。本文最下面有一个示例。
* `options` 切换按钮的选项，这个属性的默认值是`Enabled=yes&Disabled=no`，其中等号前面是文本，后面是值，可以根据需要配置。

以上所有以`-text`消息相关属性均支持 [Express 字符串](/root.js/express.md)。

## 按钮事件

按钮新增一个[服务器端事件](/root.js/server.md)`onclick+`，是按钮扩展的关键属性。属性值为按钮要执行的 PQL 语句或要请求的 API 接口，属性值格式详见[服务器端事件](/root.js/server.md)和[与数据相关的属性](/root.js/data.md)。客户端事件`onclick`依然可以使用。

另外为了配合`switch`按钮类型，新增两个客户端事件`onclick-enabled`和`onclick-disabled`，分别当按钮切换到相应的状态下触发。

客户端事件一般情况下不会用到，不过有时其他组件有时需要监听按钮的动作，参见[精简事件和表达式](/root.js/event.md)。

## 可用的选择器

扩展选择器一般用来定义事件，可用的选择器只有`1`个。

* `$s('#id')` 其中`id`前面的`#`符号不可省略。

一般情况下用不到选择器，只了解选择器的语法即可。

## 请求成功和失败

Button 扩展主要用于向后端发起请求，请求有以下几种状态：

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
* `optional-button` 无色按钮，与其他颜色按钮相反。
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
<button id="EnableButton" type="switch" options="已启用=yes&已禁用=no" value="<%= $job.enabled %>" onclick="return <%=$job.dags%> > 0;" invalid-text="至少先设置一个工作流命令才能启用调用。" onclick+="put:/api/job/switch?id=&(jobId)&enabled={value}" />
```

这个示例来自于 Master 项目后台启停用调度的按钮，主要逻辑为：只有先设置了工作流之后才能启用按钮，由`onchange`事件来判断，如果不满足启用条件，则提示文字`invalid-text`；`options`用来设置不同状态下按钮的显示文字和值；`value`属性必须设置值；`onclick+`事件用于请求后端接口，完成数据库更新。

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [与数据相关的属性](/root.js/data.md)
* [精简事件和表达式](/root.js/event.md)