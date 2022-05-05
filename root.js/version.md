# 版本和更新

**root.js** 库一般跟随 [Qross Master](/master/overview.md) 项目的功能进行更新。

## v2.4.0 (2022.4.30)

本月共 46 项更新。

* 新的标签扩展[多行文本框 TEXTAREA](/root.js/textarea.md)，新增了一些属性和方法。
* 新的标签扩展[IMG 图片](/root.js/image.md)，新增与服务器端交互的事件。
* [TEMPLATE 标签](/root.js/template.md)重写，现在是 HTMLTemplateElement 标签的扩展。
* [POPUP 标签](/root.js/popup.md)重写，POPUP 现在是一个自定义标签。
* document 新增服务器端事件支持`onready+`、`onpost+`和`onload+`，可以在 BODY 上定义。
* [INPUT 输入框](/root.js/input.md)新增事件`onkeyenter`。
* [正则表达式 RegExp](/root.js/root.md#h4) 扩展了一些更简单的方法。
* [字符串扩展 String](/root.js/root.md#h3) 新增`divide`和`do`方法。
* 标签属性`hidden`重写，解决元素在设置了`display`属性后不能正确显示和隐藏的问题。
* 一些输入组件新增和重写`readonly`、`disabled`和`enabled`属性。
* 多项优化和修复。

---
参考链接

* [root.js 概览](/root.js/overview.md)
* [root.js 基础库](/root.js/root.md)
* [root.js 历史更新](/root.js/history.md)
