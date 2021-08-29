# root.js 历史更新

## v0.6.5 (2020-12-31)

* **Cogo** 无 Javascript 的前端开发方式已开始设计。
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

## 再之前的版本无文档。