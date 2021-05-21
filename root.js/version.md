# 版本和更新

**root.js** 库一般跟随 [Qross Master](/master/overview.md) 项目的功能进行更新。

## v1.5.0 (开发中)

* [TABLE 标签扩展](/root.js/table.md)加入，在原有 DataTable 组件的基础进行了重构。
* [INPUT 标签扩展](/root.js/input.md)新增`switch`和`checkbox`类型，可用作“开关”。
* [Editor 标签编辑器](/root.js/editor.md)中关于`switch`和`checkbox`类型的编辑器移除。
* 现有组件大量优化，大量的 bug 修复。

## v1.4.0 (2021-4-30)

* [服务器端事件](/root.js/server.md)加入，比原有的`action`属性更明确，例如`onclick+="insert into ...."`。所有相关组件的核心交互属性都修改为服务器端事件。
* [精简事件](/root.js/event.md)加入，对原有的事件表达式进行了升级。
* 新组件 [DateTimePicker](/root.js/datetimepicker.md)，这是一个组合组件，用于日期和时间的选择。
* [A 标签扩展](/root.js/anchor.md)加入，主要是服务器端事件的支持。
* 弹出框组件 [Popup](/root.js/popup.md) 文档补全。
* [Select 标签扩展](/root.js/select.md)现在支持`radio`和`checkbox`类型。
* [Input](/root.js/input.md) 和 [Button](/root.js/button.md) 扩展配合服务器端事件进行了大量属性更新。
* [Input](/root.js/input.md) 新增`icon`属性，用于设置在输入框中显示的图标。新增`onmodify`事件，在值通过输入方式改变时触发。实现`onload`事件，组件加载完成时触发。
* [Coder 彩色编码]组件的提示文字配合服务器端事件进行了统一，增加`validator`属性用于 Json 内容的验证。
* [Model 模型](/root/js/model.md)中 Template 标签更新了两个方法的名称，新增`reload`方法。
* 增加`click-to-copy`属性，可以应用到任何元素上，增加点击复制文字功能。

## v1.3.0 (2021-3-24)

* [Input 扩展](/root.js/input.md)新增扩展类型：`integer`，`password`，`idcard`，`mobile`，`name`等，以及`check`、`warning-text`、`autosize`等扩展属性。
* [Input 扩展](/root.js/input.md)新增属性`required`（原生）、`check`、`pass-when`、`warning-text`、`autosize`、`hint`、`callout`等属性，以及移除`prevent-injection`属性（改为通过后端接口防止 SQL 注入）。
* [Button 扩展](/root.js/button.md)新增`success-when`属性（1.4 中已移除）。
* [Input 扩展](/root.js/input.md)和 [Button 扩展](/root.js/button.md)现在所有提示文字属性支持 [Express 字符串](/root.js/express.md)。
* [Express 字符串]现在 Javascript 占位符默认支持 `data`、`value`、`text` 三个变量；现在所有占位符支持以`%`结尾来进行 URL 编码。
* 修复各个组件的多个 bug.

## v1.2.0 (2021-2-28)

* [**Button** 扩展](/root.js/button.md) 现在与其他表单元素联动。
* [**Input** 扩展](/root.js/input.md) 加入，扩展了原生 INPUT 标签的功能。
* [**Select** 扩展](/root.js/select.md) 加入，为 SELECT 增加更多功能。SelectButton 组件整合进 Select。
* [**SPAN** 标签](/root.js/model.md) 现在支持数据加载功能。
* [**Layout** 布局组件](/root.js/layout.md) 布局组件加入，目前只有一个简单的 Panel 功能。
* [**Editor** 编辑器组件](/root.js/editor.md)升级，移除`method`和`data`属性，现在`action`属性支持 PQL 语句。
* [**Coder** 彩色编码组件](/root.js/coder.md)移除`method`和`data`属性，现在`action`属性支持 PQL 语句。
* `root.css` 现在只包含通用性样式和常用标签样式，不常用标签样式转移到自己的样式文件中，如`root.treeview.css`。
* [事件表达式](/root.js/event.md)加入，目的是尽可能的不写 Javascript 代码。
* `$s`选择器现在可以选择扩展标签，仅可使用`name`属性进行选择，如`$s('#Button1')`。
* `$a`选择器现在可以选择扩展标签，仅可使用`name`属性进行选择，如`$a('#Input1,#Input2,#Input3')`。

## v1.1.0 (2021-1-31)

* **root.js 组件库** 从 Master 项目中分离，独立成项目。
* [**root.js 基础库**](/root.js/root.md) 重新整理，文档已补全。
    + 增加新的选择器`$t`，可选择单个自定义标签
    + 增加新的选择器`$c`，可选择多个自定义标签
    + `TAKE` 方法移除`path`属性，整合进`data`属性中。
    + `ajax.fill(el)` 方法移除。
    + `String`扩展增加`fill`方法，可以将字符串填充到元素中。
    + 移除所有组件的`Tag$(name)`方法，统一使用`$listen(name)`。
    + 修正多个 bug，一些不常用方法移除
* [**Model 数据加载模型**](/root.js/model.md) 所有占位符已重新设计，文档已补全。
    + `model.js` 现在已经支持`<O>`标签。
        ```html
        <o>select title from table where id=#{id} -> frist cell</o>
        ```
    * `model.js` 所有`path`属性移除。
* [自定义标签属性`data`](/root.js/data.md)现在支持直接写 PQL 语句。
* [**BackTop**](/root.js/backtop.md) 增加自定义元素功能和事件，文档已补全。
* **TreeView** 显示引导线`lines`逻辑调试通过，移除`$path`属性。
* **Button** 多个无用属性移除，`setText`方法修改为属性。
* **SelectButton** 选项类 ButtonOption 增加`show`和`hide`属性，用于在切换时显示或隐藏元素。
* `cogo.js` 移除，核心功能整合进`root.js`。

## v0.6.5 (2020-12-31)

* `cogo.js` 已开始设计。
* `calender.js` 已禁用的日期不再可以设置特殊工作日和休息日。
* `coder.js` 现在在 PC 端也默认回行。

## v0.6.4 (2021-9-30)

* [Calendar 日历组件](/root.js/calendar.md)大幅更新，增加了大量功能。
    * 通过`months="12"`可以设置整年模式，可切换上一年和下一年或任意选择年份。
    * 通过`months="n"`可以设置多月模式，可切换上一月和下一月或任意选择年月。
    * 已经可以显示农历，需要接口支持。
    * 可以显示特殊工作日和休息日（法定节假日）角标，法定节假日需要在数据库中进行设置。
    * 不同模式下增加“今天”、“本周”、“OK”等按钮。
    * 样式基本重写，适配移动端。
* [Clock 文字时钟组件](/root.js/clock.md)加入，可进行交互选择。
* 可通过[Popup 弹出框组件](/root.js/popup.md)组合[Calendar](/root.js/calendar.md)和[Clock](/root.js/clock.md)实现日期时间选择。
* [DateTime 类](/root.js/datetime.md)配合 [Calendar](/root.js/calendar.md) 和 [Clock](/root.js/clock.md) 大量更新。
* 修复 [root.js 基本库](/root.js/root.md)的一些问题。
* 修复 [Animation 动画库](/root.js/animation.md)的一些问题。

## v0.6.3 (2021-7-31)

* [Coder](/root.js/coder.md) 编码器加入，封装了`CodeMirror`，实现 PQL 及其他语言的彩色编码。
* [TreeView 树形目录组件](/root.js/treeview.md)的`TreeNode`节点类新增`expandOnRender`属性，用于确定首次加载时是否自动展开节点。