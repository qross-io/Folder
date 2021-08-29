# root.js 前端标签库

**root.js** 是一个为了提高页面的开发效率而开发的一套前端标签库，包括多个基础库和标签库。基础库用于常用 DOM 操作、数据加载等功能，标签库提供多个标签扩展或复合组件。root.js 的目标是尽可能的不写 Javascript 代码，通过 HTML 标签和属性即可实现页面上的各种交互功能。

root.js 由原生 Javascript 编写，可以应用到 Spring Boot、PHP 等开发中，应该不适用于 Vue 或 React 等流行的前端框架。

框架中有的组件可以追溯到 2001 年，期间经历过多次修改和变迁，在 2012 年时已经有大约 10 个组件。在作者工作内容由技术转为管理的很多年中，标签库没有更新。2018 年由于要实现 Qross 系统的管理后台，重拾整个框架。更多标签和组件会不断加入到库中。

标签库源码地址 <https://github.com/qross-io/root.js>，每月月底更新版本。

## 基础库

* [root.js](/root.js/root.md) 基础功能类似于`jQuery`，同时提供很多基础的操作并为其他组件服务。
* [与数据相关的标签扩展属性](/root.js/data.md)，让标签具有与后端交互的能力。
* [Express 字符串](/root.js/express.md) 提供快速串接字符串和显示元素标签内容的方法。
* [Model 数据加载模型](/root.js/model.md) 提供从后端获取数据的能力，支持 FOR 循环、IF 判断等自定义标签。
* [服务器端事件](/root.js/server.md) 为关键事件提供与后端快速交互的能力。
* [事件表达式](/root.js/event.md) 用最简单的方法绑定事件。
* [DateTime 日期时间类](/root.js/datetime.md) 比原生的日期时间类功能更多。

## 标签库

* [BackTop 返回顶部](/root.js/backtop.md)  在页面右下角显示返回顶部的按钮，元素和内容可自定义。
* [Button 按钮扩展](/root.js/button.md) 扩展了按钮功能，主要是后端交互的功能。
* [Calendar 多功能日历组件](/root.js/calendar.md) 支持单选、周选、多选等功能，支持农历和特殊工作日和休息日显示。
* [Clock 文字时钟组件](/root.js/clock.md) 支持时、分、钟的选择，并支持定义下拉选项。
* [Coder 彩色编码组件](/root.js/coder.md) 在 CodeMirror 上进行了一些扩展。
* [Input 文本框扩展](/root.js/input.md) 扩展了文本输入框的功能，支持验证和提醒。
* [DIV 布局增强](/root.js/div.md) 扩展了 DIV 以方便页面布局。
* [Select 选择器组件](/root.js/select.md) 扩展了原生的 SELECT 标签，提供更多的选择项风格。

更多组件请查看左侧项目目录。

---
参考链接

* [root.js 基础库](/root.js/root.md)