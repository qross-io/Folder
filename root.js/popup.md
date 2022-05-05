# Popup 页内弹出框

Popup 提供页面弹出框功能，是一个自定义元素。Popup 可以理解为是一个浮动 DIV 层，在元素内可以嵌套任何 HTML 元素。标签名 `POP-UP`，中横线必须有，对应的元素类为 HTMLPopupElement。

```html
<pop-up>......</pop-up>
```

这个组件第一个版本诞生于 2006 年，期间已经过多次演进。

## 属性、方法和事件

Popup 可用的属性如下：

* `type` 显示模式，可选值有：`window` 窗口模式，`sidebar` 边栏模式，`menu` 菜单模式。显示模式配合`position`属性使用，默认值为`window`。
* `modal` 是否使用模态，默认`true`。模态下在 Popup 打开时 Popup 之外的元素不可操作。
* `position-x` 指弹出框相对于 X 轴的位置，可选值有`left`、`center`、`right`、`event`和具体的数字。数字表示页面的像素位置。默认值为`center`。更详细的信息在下文会提到。
* `position-y` 指弹出框相对于 X 轴的位置，可选值有`top`、`middle`、`bottom`、`event`和具体的数字。数字表示页面的像素位置。默认值为`middle`。更详细的信息在下文会提到。
* `offsetX`或`offset-x` 打开时 X 轴的偏移距离，单位为像素。
* `offsetY`或`offset-y` 打开时 Y 轴的偏移距离，单位为像素。
* `open-button` 设置打开按钮的 id 或其他可用的选择器，默认值为`#{popup-id}_OpenButton`，如`#Popup1_OpenButton`。
* `close-button` 设置关闭按钮的 id 或其他可用的选择器，默认值为`#{popup-id}_CloseButton`，如`#Popup1_CloseButton`。
* `confirm-button` 设置确认按钮的 id 或其他可用的选择器，默认值为`#{popup-id}_ConfirmButton`，如`#Popup1_ConfirmButton`。
* `cancel-button` 设置确认按钮的 id 或其他可用的选择器，默认值为`#{popup-id}_CancelButton`，如`#Popup1_CancelButton`。
* `reference` 菜单模式下的参考元素。
* `hidings` 打开时需要隐藏的元素，使用选择器设置，支持多个元素。
* `disable-scrolling` 打开时是否禁用滚动条，默认为`false`，边栏械下默认为`true`。
* `mask-color` 模态背景的颜色，默认为`#999999`。
* `mask-opacity` 模态背景的透明度，可选值`0 ~ 10`，`0`为完全透明，`10`为完全不透明，默认值`3`。

可用的方法有：

* `open()` 打开对话框，触发`onopen`事件。
* `confirm()` 点击确认按钮并关闭对话框，触发`onconfirm`事件。
* `close()` 点击关闭按钮关闭对话框，触发`onclose`事件。
* `cancel()` 点击取消按钮关闭对话框，触发`oncancel`事件。

可用的事件有：

* `onopen` 当 Popup 打开时触发，事件函数参数为当前事件变量`ev`，如`function(ev) { ... }`，支持`return false`取消打开。
* `onclose` 当 Popup 关闭时触发，事件函数参数为当前事件变量`ev`，支持`return false`。
* `onconfirm` 当 Popup 确定时触发，事件函数参数为当前事件变量`ev`。这个事件最重要，支持`return false`。
* `oncancel` 当 Popup 取消时触发，事件函数参数为当前事件变量`ev`，支持`return false`。
* `onshow` 当 Popup 显示后触发，打开算显示。
* `onhide` 当 Popup 隐藏后触发，关闭、确认、取消都算隐藏。

示例如下：

```javascript
$('#Popup1').on('confirm', function(ev) {
    //......
});
```

## 窗口模式

属性`position-x`和`position-y`的位置针对整个窗口，如分别设置为`left`和`top`让整个窗口显示在窗口的左上角。

```html
<pop-up id="Popup1" type="window" position-x="center" position-y="top">
    <div>
        这个是一个 Popup。
    </div>
    <div>
        <button id="Popup1_ConfirmButton">OK</button>
        <button id="Popup1_CancelButton">Cancel</button>
    </div>
</pop-up>
```

## 边栏模式

边栏模式下，`position-x`设置为`left`、`right`时为左右边栏，`position-y`设置为`top`和`bottom`时为上下边栏。上下边栏会自动调整 Popup 宽度为整个窗口宽度；左右边栏会自动调整 Popup 高度为整个窗口高度。X 轴设置优先于 Y 轴，即只要 X 轴不是`center`时，Y 轴默认为`middle`，其他设置值无效。边栏模式下默认禁用滚动条。

```html
<pop-up id="Popup1" type="sidebar" position-x="right"></pop-up>
```

## 菜单模式

菜单模式下，`position-x`和`position-y`的位置不再相对于窗口，而是`reference`属性设置的参考元素。`left`相对于参考元素左对齐，`center`与参考元素居中对齐，`right`相对于参考元素右对齐，`top`让 Popup 显示在参考元素上方，`bottom`让 Popup 显示在参考元素下方，`middle`设置无效。

```html
<pop-up id="Popup1" type="menu" position-x="left" position-y="bottom"></pop-up>
```

## 对话框

基于 Popup，标签库实现了对应三种 window 对话框的新弹出框，用于标签库内部使用。三种对话框均返回一个 Popup 对象。

* `$root.alert(message, confirmButton = 'OK', title = 'Message')` 显示警告对话框。
* `$root.confirm(message, confirmButton = 'OK', cancelButton = 'Cancel', title = 'Confirm')` 显示确认对话框。
* `$root.prompt(message, input = 'text', confirmButton = 'OK', cancelButton = 'Cancel', title = 'Prompt')` 显示输入提示对话框。

对话的每个参数除可传入字符串外，都支持传入对象类型以对每一项进行更多设置。

* `message` 对象属性有`text`和`class`。
* `confirmButton` 对象属性有`text`、`class`、`icon`。
* `cancelButton` 对象属性有`text`、`class`、`icon`。
* `input` 对象属性支持所有 [INPUT 标签](/root.js/input.md)的属性。

## 动画属性

Popup 支持自定义打开和关闭时的动画，动画元素由位置、缩放比例和透明度三个元素构成，不支持旋转角度设置。可用的属性有：

* `opening-from-position` 打开动画的起始位置，默认值`x(0).y(0)`，表示相对于最终位置不做任何偏移，如`x(-100).y(0)`表示相对于最终位置向左偏移`100`像素。
* `opening-to-position` 打开动画的起始位置，默认值`x(0).y(0)`。
* `opening-from-opacity` 打开动画的起始透明度，默认值`100%`，即不透明。`0%`表示完全透明。
* `opening-to-opacity` 打开动画的结束透明度，默认值为`100%`。
* `opening-from-scale` 打开动画的起始缩放比例，默认值为`100%`，即不缩放。可以设置超过`100%`的值，也可以分别为 X 方向和 Y 方向分别设置比例，如`100%.50%`。
* `opening-to-scale` 打开动画的结束缩放比例，默认值为`100%`。
* `opening-duration` 打开动画的持续时间，默认值为`0.8s`，单位也可以选择秒`s`或毫秒`ms`，比如`1s`、`800ms`。
* `closing-from-position` 关闭动画的起始位置，默认值`x(0).y(0)`，表示相对于最终位置不做任何偏移，如`x(-100).y(0)`表示相对于最终位置向左偏移`100`像素。
* `closing-to-position` 关闭动画的起始位置，默认值`x(0).y(0)`。
* `closing-from-opacity` 关闭动画的起始透明度，默认值`100%`，即不透明。`0%`表示完全透明。
* `closing-to-opacity` 关闭动画的结束透明度，默认值为`100%`。
* `closing-from-scale` 关闭动画的起始缩放比例，默认值为`100%`，即不缩放。可以设置超过`100%`的值，也可以分别为 X 方向和 Y 方向分别设置比例，如`100%.50%`。
* `closing-to-scale` 关闭动画的结束缩放比例，默认值为`100%`。
* `closing-duration` 关闭动画的持续时间，默认值为`0.8s`，单位也可以选择秒`s`或毫秒`ms`，比如`1s`、`800ms`。
        

---
参考链接

* [Calendar 日历组件](/root.js/calendar.md)
* [Clock 数字时钟组件](/root.js/clock.md)
* [Editor 文本编辑](/root.js/editor.md)