# Clock 文字时钟组件

**Clock** 组件的功能相对简单，支持时、分、秒的选择，并支持自定义下拉选项。

## 下载

[点击下载Clock组件及相关依赖文件](http://www.qross.cn/download?file=clock.zip)

## 使用

Clock依赖库或组件列表如下：

* [root.js 基本库](/root.js/root.md)
* [Animation 动画库 root.animation.js](/root.js/animation.md)
* [DateTime 日期时间类 root.datetime.js](/root.js/datetime.md)
* Clock 组件默认样式 `root.clock.css`

## 示例

<script type="text/javascript" src="/root/root.js"></script>
<script type="text/javascript" src="/root/root.animation.js"></script>
<script type="text/javascript" src="/root/root.datetime.js"></script>
<script type="text/javascript" src="/root/root.clock.js"></script>
<link href="/root/root.clock.css" rel="stylesheet" type="text/css" />

### 文字时钟

```html
<clock id="clock1"></clock>
```
在不指定任何选项的情况下即一个会跳秒的文字时钟。

<clock id="clock1"></clock>

### 可选择小时和分钟

```html
<clock id="clock2" hour-interval="1" minute-interval="1" value="HH:mm:00"></clock>
```
一般情况下“秒”不需要选择，`value`属性可以设置时钟的初始值。

<clock id="clock2" hour-interval="1" minute-interval="1" value="HH:mm:00"></clock>

### 可选择`2`的倍数的小时和`10`的倍数的分钟
```html
<clock id="clock3" hour-interval="2" minute-interval="10" option-frame-side="upside" value="HH:00:00"></clock>
```
通过调整小时和分钟的间隔，设置出不同的选项。下拉框在上方显示。

<clock id="clock3" hour-interval="2" minute-interval="10" option-frame-side="upside" value="HH:00:00"></clock>

### 指定选择项
```html
<clock id="clock4" hour-interval="8,12,14,18" minute-interval="0,30" second-visible="no" option-frame-align="left" value="HH:00:00"></clock>
```
没有显示秒，也可以设置不显示其他项。设置了下拉框左对齐。

<clock id="clock4" hour-interval="8,12,14,18" minute-interval="0,30" second-visible="no" option-frame-align="left" value="HH:00:00"></clock>

## 标签和类

### 标签属性

标签属性可以在自定义标签`<clock>`上进行设置。

* `id`或`name` Clock在页面上的唯一名称。
* `hour-interval` 小时间隔，可输入数字或逗号分隔的数字列表，数值区间为`0~23`。如果是数字，则按间隔计算从`0`点开始至`23`点中间的各个小时点；如果是逗号分隔的数字列表，则只在下拉选项中显示这个列表中的数字。当设置为`0`时，则不显示下拉列表。默认值为`0`。
* `minute-interval` 分钟间隔，数值区间为`0~59`。计算方式同小时。默认值为`0`，即不显示下拉选项。
* `second-inteval` 秒间隔，数值区间为`0~50`，默认值为`0`。计算方式同小时和分钟。这个选项一般不需要设置。
* `value` 时钟的初始值，格式为`HH:mm:ss`，可用`HH`代替当前时间的小时，`mm`代替当前时间的分钟，`ss`代替当前时间的秒钟。如`HH:mm:00`，设置为当前时间的小时和分钟，但是秒钟设置为`0`。
* `hour-visible` 是否显示小时部分，默认为`true`。
* `minute-visible` 是否显示分钟部分，默认为`true`。
* `second-visible` 是否显示秒钟部分，默认为`true`。
* `frame-class` 整个`DIV`框体的样式，默认值为`-clock`。
* `hour-class` 小时部分的`INPUT`样式，默认值为`-clock-hour`。
* `minute-class` 分钟部分的`INPUT`样式，默认值为`-clock-minute`。
* `second-class` 秒钟部分的`INPUT`样式，默认值为`-clock-second`。
* `colon-class` 冒号的样式，也是一个`INPUT`元素，默认值为`-clock-colon`。
* `option-frame-class` 选项`DIV`框体的样式，默认值为`-clock-option-frame`。
* `option-class` 选项框体中选项`TD`的样式，默认值为`-clock-option`。
* `checked-option-class` 选项框中已选中项`TD`的样式，默认值为 `-clock-checked-option`。


### 选择器

因为`<clock>`属于自定义HTML标签，不能使用传统的选择器。可用的选择器有两个：

* `$clock('name')` 这个选择器返回`Clock`对象，可以访问Clock对象的属性和方法。但是需要在页面加载完成并且标签解析之后使用。如
    ```javascript
    $finish(function() {
        $clock('clock1').time = '12:00:00';
    });
    ```
* `Clock$('name')` 这个主要为`Clock`对象添加事件，不需要等待页面加载完成，可随时调用。如
    ```javascript
    Clock$('name').on('HourChanged', function(hour) {
        console.log(hour);
    });
    ```

### 属性

Clock对象只有几个简单的属性：

* `time` 获取或设置文字时钟的时间，格式`HH:mm:ss`，如`12:30:00`。
* `hour` 获取或设置时钟的小时。
* `minute` 获取或设置时钟的分钟。
* `second` 获取或设置时钟的秒钟。

### 方法

Clock对象没有可用的方法。

### 事件

* `onHourChanged` 当小时改变后触发，传递`hour`参数
* `onMinuteChanged` 当分钟改变后触发，传递`hour`参数
* `onSecondChanged` 当秒钟改变后触发，传递`second`参数
* `onTimeChanged` 当任何时间位改变后触发，传递`time`参数，格式`yyyy-MM-dd`

例如：
```javascript
Clock$('clock2').on('TimeChanged', function(time) {
    console.log(time);
});
```

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Animation 动画库](/root.js/root.md)
* [DateTime 日期时间类](/root.js/datetime.md)
* [Calendar 日历组件](/root.js/calendar.md)