# root.js 前端组件库

**root.js** 是一个为了提高页面的开发效率而开发的一套前端组件库，包括多个基础库和组件库。基础库用于常用 DOM 操作、数据加载等功能，组件库提供多个复合组件。root.js 的目标是尽可能的少写 Javascript 代码，甚至在不需要交互的场景，都无需要编写 Javascript 代码。

root.js 库是 [Cogo 计划](/cogo/overview.md)的关键部分。

## 基础库

* [**root.js**](/root.js/root.md) 基础功能类似于`jQuery`，同时提供很多基础的操作并为其他组件服务。
* [**标签扩展属性 data**](/root.js/data.md)，让标签具有与后端交互的能力。
* [**DateTime 日期时间类**](/root.js/datetime.md) 比原生的日期时间类功能更多。
* [**Express 字符串**](/root.js/express.md) 提供快速串接字符串和显示元素标签内容的方法。
* [**Model 数据加载模型**](/root.js/model.md) 提供从后端获取数据的能力，支持 FOR 循环、IF 判断等自定义标签。
* [**事件表达式**](/root.js/event.md) 用最简单的方法绑定事件。

## 组件库

* [**BackTop 返回顶部**](/root.js/backtop.md)  在页面右下角显示返回顶部的按钮，元素和内容可自定义。
* [**Button** 按钮扩展](/root.js/button.md) 扩展了按钮功能，主要是后端交互的功能。
* [**Calendar 多功能日历组件**](/root.js/calendar.md) 支持单选、周选、多选等功能，支持农历和特殊工作日和休息日显示。
* [**Clock 文字时钟组件**](/root.js/clock.md) 支持时、分、钟的选择，并支持定义下拉选项。
* [**Input 文本框扩展**](/root.js/input.md) 扩展了文本输入框的功能，支持验证和提醒。
* [**Layout 布局组件**](/root.js/layout.md) 扩展了 DIV 以方便页面布局。
* [**Select 选择器组件**](/root.js/select.md) 扩展了原生的 SELECT 标签，提供更多的选择项样式。