# Button 按钮扩展

组件库对 BUTTON 标签进行了扩展，主要是为了实现与后端进行快速交互。

## 使用

需要引入两个文件，注意`root.button.js`文件的引入要在其他表单组件如`root.input.js`之后。

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.button.js"></script>
```

<script type="text/javascript" src="@/root.input.js"></script>
<script type="text/javascript" src="@/root.button.js"></script>

## 标签示例

```html
<button id="Button1" action="pql|api" watch="#TextBox1,#TextBox2" confirm-text="" action-text="" success-text="" failed-text="" exception-text="" jumping-text="" jump-to="url" enabled-class="" disabled-class="" onclick="" onActionSuccess="" onActionFail="" onActionException="" onActionComplete="">按钮</button>
```

可用扩展属性如下：

* `action` 按钮要执行的 PQL 语句或 API 接口，见 [data 属性](/root.js/data.md)
* `watch` 用于组件监听，下面在“组件监听”中单独介绍这个属性。
* `confirm-text` 如果设置了这个属性，则在点击按钮时显示确认提示框，提示框里显示这个属性的文字。
* `action-text` 执行`action`请求时的提示文字，替换按钮上的文字。
* `success-text` 请求成功后的提示文字，替换按钮上的文字，返回值可以识别成正确的布尔值，见下面“请求成功与失败”中的详细说明。
* `failed-text` 请求失败后的提示文字，替换按钮上的文字，见下面“请求成功与失败”的说明。
* `exception-text` 请求发生异常后的提示文字，替换按钮上的文字，“异常”是指接口或 PQL 语句发生错误，错误会被打印到控制台。
* `jump-to` 请求成功后的跳转 URL 地址。
* `jumping-text` 跳转过程中的提示文字。
* `enabled-class` 启用状态下的样式，默认值`normal-button blue-button`
* `disabled-class` 按钮在禁用状态下的样式，默认值`normal-button optional-button`。

## 按钮事件

事件一般情况下不需要设置，不过有时其他组件有时需要监听按钮的动作，参见[事件表达式](/root.js/event.md)。可用事件如下：

* `onActionSuccess` 当请求成功时触发。事件函数`function(data)`，参数为返回的数据。
* `onActionFail` 当请求失败时触发。事件函数`function(data)`，参数为返回的数据。
* `onActionException` 当请求发生错误时触发。事件函数`function(error)`，参数为错误信息。
* `onActionComplete` 当请求完成后触发。事件函数`function()`，无参数。

示例：

```javascript
$listen('Button1').on('actionException', function(err) {
    console.log(err);
})

$s('#Button1').on('ActionSuccess', function(data) {
    //...
}).on('actionfail', function(data) {
    //...
});

$s('#Button1').onActionComplete = function() {
    //...
}

```

上例中，三种方式都可以绑定事件，建议使用`$listen`和`on`方法绑定事件。第一种和第二种方式事件名称不区分大小写，可使用多次绑定多个事件；但第三种方式区分大小写，只能绑定一个事件。两种方法绑定的事件互相不影响，优先执行第三种方式。

## 可用的选择器

扩展选择器一般用来定义事件，可用的选择器只有`1`个。

* `$s('#id')` 其中`id`前面的`#`符号不可省略。

一般情况下用不到选择器，只了解选择器的语法即可。

## 请求成功和失败

Button 扩展主要用于向后端发起请求，请求有以下几种状态：

* `success` 成功状态，表示返回值符合预期，依照返回值类型确定。默认值为成功状态，即识别不出是失败状态，即是成功状态。
    + 布尔值，`true`
    + 字符串，可识别为`true`的字符串，如`yes`、`1`、`true`、`on`等，不区分大小写
    + 数字，大于`0`
    + 数组或对象，不为空
* `failed` 失败状态，表示返回值不符合预期，依照返回值类型确定。
    + 布尔值，`false`
    + 字符串，可识别为`false`的字符串，如`no`、`0`、`false`、`off`等，不区分大小写
    + 数字，小于等于`0`
    + 数组或对象，为空
* `exception` 请求异常状态，PQL 语句或接口出错。

大多数情况下，按钮行为都是向数据库插入或更新数据，插入数据可以返回最新数据的自增`id`，更新数据可以返回受影响的行数。

## 组件监听

Button 一般情况下会配合其他表单组件使用，如文本框等。Button 提供了对其他表单组件进行监听的功能，已在表单输入未完成和完成时作出正确的响应，决定是否禁用按钮。这个响应是自动的，一般情况下也不需要专门配置。

```html
<button id="Button1" action="insert into table1 (title, views) values ('$(#Title)', $(#Views))">Add</button>
<button id="Button2" watch="#Title,#Views">Add</button>
<button id="Button3" action="insert into table1 (title, views) values ('$(#Title)', $(#Views))" watch="#Source">Add</button>
```

监听其他组件有两种方式。第一是设定`action`属性，页面加载时会自动分析`action`属性中相关的组件是否为必填项`required`，Button 会自动将必填组件加入监听列表。第二是设定`watch`属性，Button 会自动将`watch`属性中的组件加入监听列表，只能按照组件的名字或 id 进行监听，多个组件之间使用逗号`,`分隔。当只使用一种方式不能满足监听要求时，也可以两种方式混合使用。上例中，假如`#Title`，`#Views`，`#Source`都是必填项，则按钮必须在这 3 个表单项的输入值都正确时才会启用。

试试下面的例子，文本框输入值时 Button 才能启用。

<input id="Input1" type="text" required-text="Please input something." invalid-text="Please input 3 characters at least." minlength="3" size="30" />

<button watch="#Input1"> Submit </button>

示例代码如下：

```html
<input id="Input1" type="text" required-text="Please input something." invalid-text="Please input 3 characters at least." minlength="3" size="30" />
<button watch="#Input1"> Submit </button>
```

## 链接按钮

除了后端请求以外，还可以把按钮当作链接使用。

```html
<button href="/index.html">返回首页</button>
```

这个功能不需要引入`root.button.js`文件。

## 可用的按钮样式

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
<button class="normal-button orange-button" disabled-class="normal-button gray-button">
```

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [data 属性](/root.js/data.md)
* [事件表达式](/root.js/event.md)