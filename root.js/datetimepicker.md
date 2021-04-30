# 日期时间选择器

日期时间选择器是几个组件的综合运用，包括[页面弹出框 Popup](/root.js/popup.md)，[日历 Calendar](/root.js/calendar) 和[数字时钟 Clock](/root.js/clock.md) 三个组件。

```html
<input type="datetime" value="now" />
<span type="datetime">today</span>
```

日期时间选择框支持以上两种形式，一种是输入框形式，INPUT 输入框`type`属性要设置为`datetime`（区别于原生属性值`datetime-local`），另一种是文本标签 SPAN 形式。输入框形式可以使用在表单中，可以使用 [INPUT 标签扩展](/root.js/input.md)的各种验证功能，但不可手工输入值。

<script type="text/javascript" src="@/root.animation.js"></script>
<script type="text/javascript" src="@/root.datetime.js"></script>
<script type="text/javascript" src="@/root.datetimepicker.js"></script>
<script type="text/javascript" src="@/root.calendar.js"></script>
<script type="text/javascript" src="@/root.clock.js"></script>
<script type="text/javascript" src="@/root.popup.js"></script>
<link href="@/css/root/iconfont.css" rel="stylesheet" type="text/css" />
<link href="@/css/root/calendar.css" rel="stylesheet" type="text/css" />
<link href="@/css/root/clock.css" rel="stylesheet" type="text/css" />
<style type="text/css">
input { font-size: 1rem; }
</style>

见下面示例：

<input type="datetime" clock-minute-interval="30" clock-second-interval="0" value="today" size="30" />

如上例，选择器表现为一个文本框，右面显示一个日历图标，点击图标会显示选择对话框，选择日期时间后，点击“确定”按钮会更新文本框的值。

可用的属性有：

* `title` 弹出框的标题栏内容，默认空。
* `tip` 弹出框的详细提示文字，默认空。
* `calendar-{attr}` 日历属性设置，可设置 [Calendar 日历组件](/root.js/calendar.md)相关的属性，必须以`calendar-`开头，比如`calendar-week-names="一,二,三,四,五,六,日"`，这里日历组件的`mode`属性无效。
* `clock-{attr}` 数字时钟属性设置，可设置 [Clock 数字时钟组件](/root.js/clock.md)相关的属性，必须以`clock-`开头，比如`clock-minute-interval="10"`。
* `popup-{attr}` 弹出框属性设置，可设置 [Popup 弹出框组件](/root.js/clock.md)相关的属性。日期时间选择器在

可用的事件只有一个，但其他 INPUT 或 SPAN 原生事件均支持：

```html
<input id="DateTime1" type="datetime" onpick="alert(this.value)" />
```

或

```javascript
$listen('DateTime1').on('pick', function(value) {
    //......
});
```

日期时间选择器相关文件有：

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.animation.js"></script>
<script type="text/javascript" src="/root.datetime.js"></script>
<script type="text/javascript" src="/root.datetimepicker.js"></script>
<script type="text/javascript" src="/root.calendar.js"></script>
<script type="text/javascript" src="/root.clock.js"></script>
<script type="text/javascript" src="/root.popup.js"></script>
<link href="/css/root/calendar.css" rel="stylesheet" type="text/css" />
<link href="/css/root/clock.css" rel="stylesheet" type="text/css" />
<link href="/css/root/iconfont.css" rel="stylesheet" type="text/css" />
```

---
参考链接

* [日期时间类 DateTime](/root.js/datetime.md)
* [Calendar 日历组件](/root.js/calendar.md)
* [Clock 文件时钟](/root.js/clock.md)
* [Popup 弹出框](/root.js/popup.md)
* [Express 字符串](/root.js/express.md)