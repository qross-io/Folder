# 版本和更新

**root.js** 库一般跟随 [Qross Master](/master/overview.md) 项目的功能进行更新。

## v2.5.0 (2022.5.31)

大版本，共 86 项更新，主要是底层优化，多个组件重写。本月所有更新全部为 [TREEVIEW 树形目录](/root.js/treeview.md)版本升级做准备。

* [基础库](/root.js/root.md)大量更新。
    + HTMLElement 扩展新增`css`属性，获取元素上设置的所有样式，包括`className`属性中设置的样式。
    + HTMLElement 扩展属性`visible`现在判断透明度规则。
    + HTMLElement 扩展属性`debug`用于开发调试，会自动在控制台打印服务器端接口的调用信息。
    + HTMLElement 重写方法`getAttribute`，现在支持中横线`-`分隔的属性名。
    + HTMLElement 新增方法`$child`和`$$children`，用于仅选择自己的第一级子元素。
    + Array 扩展新增`toObject(func)`方法。
    + Number 新增扩展方法`min(num)`和`max(num)`。
    + String 扩展方法`$includes`和`$remove`方法被移除，`$length`方法修改为`unicodeLength`属性。
    + String 扩展方法`$trim`修改为`trimPlus`，是`trim`方法的增强版本。
    + String 新增方法`joint(left, right)`，是`trimPlus`的逆操作。
    + String 新增方法`$()`和`$$()`，将字符串做为选择器参数，如`'#id'.$()`等价于`$('#id')`。
    + String 一些方法改为属性。
    + Json 被移除，相关方法改为 JSON 扩展。
    + 新增全局变量`$else`，记录上一个`if`方法前的值，例如标签、字符串都有`if`方法，方便进行链式编码。
    + 自定义事件现在支持 Camel 格式的事件名。
    + `$random()`修改为`Math.randomNext()`，`$shuffle()`修改为`String.shuffle()`，其他以`$`开头和方法和属性逐渐被移除或改名。
* [数据占位符](/root.js/holder.md)格式统一。
* [Express 字符串](/root.js/express.md)现在支持对 URL 地址中的常量编码，在常量后加`%`即可。对应方法`str.replaceHolder()`。
* [Model 数据加载模型](/root.js/model.md)全部重写，现在类名为`HTMLModelElement`，所有数据标签占位符格式简化并统一。
* [Template 数据模板](/root.js/template.md)大量优化，统一占位符格式。新增只读属性`standalone`标识是否独立模板。
* [FOR 循环](/root.js/for.md)重写，现在类名为`HTMLForElement`，统一占位符格式。
* [IF 判断](/root.js/if.md)重写，现在类名为`HTMLIfElement`。
* [BUTTON 按钮](/root.js/button.md)扩展大量优化。
    + 新增属性`enabledOnFailure`和`enabledOnException`。
* [INPUT 输入框](/root.js/input.md)扩展大量优化。
* [Coder 彩色编码](/root.js/coder.md)代码升级，类名及代码结构修改；CodeMirror主文件版本由`5.55.5`升级到`5.65.4`；新增 Message 支持。
* [BackTop](/root.js/backtop.md)现在是自定义标签`BACK-TOP`
* [Animation 动画支持](/root.js/move.md)新增`move`和`resetAnimation`方法。Message 增加`hideLast()`方法。
* [Popup 弹出框](/root.js/popup.md)属性、事件重新整理。
* 底层改动巨大。
    + 原生标签（自定义）事件大改，各种事件相关方法重新整理。
    + 自定义标签事件与原生标签事件统一。
    + FOR 标签的嵌套加载逻辑升级。
    + `$s`和`$a`逻辑被简化。
    + 原自定义标签类、方法及相关逻辑全部升级，旧逻辑被移除。    
    

## v2.4.0 (2022.4.30)

本月共 46 项更新。

* 新的标签扩展[多行文本框 TEXTAREA](/root.js/textarea.md)，新增了一些属性和方法。
* 新的标签扩展[IMG 图片](/root.js/image.md)，新增与服务器端交互的事件。
* [TEMPLATE 标签](/root.js/template.md)重写，现在是 HTMLTemplateElement 标签的扩展。
* [POPUP 标签](/root.js/popup.md)重写，现在是自定义标签`POP-UP`。
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
