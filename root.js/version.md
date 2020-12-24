# 版本和更新

**root.js** 库一般跟随[Qross Master](/master/overview.md)的功能进行更新。相关文档会慢慢补全。

## v0.6.5

* `cogo.js` 已开始设计。
* `calender.js` 已禁用的日期不再可以设置特殊工作日和休息日。
* `coder.js` 现在在 PC 端也默认回行。

## v0.6.4

* [Calendar日历组件](/root.js/calendar.md)大幅更新，增加了大量功能。
    * 通过`months="12"`可以设置整年模式，可切换上一年和下一年或任意选择年份。
    * 通过`months="n"`可以设置多月模式，可切换上一月和下一月或任意选择年月。
    * 已经可以显示农历，需要接口支持。
    * 可以显示特殊工作日和休息日（法定节假日）角标，法定节假日需要在数据库中进行设置。
    * 不同模式下增加“今天”、“本周”、“OK”等按钮。
    * 样式基本重写，适配移动端。
* [Clock文字时钟组件](/root.js/clock.md)加入，可进行交互选择。
* 可通过[Popup弹出框组件](/root.js/popup.md)组合[Calendar](/root.js/calendar.md)和[Clock](/root.js/clock.md)实现日期时间选择。
* [DateTime类](/root.js/datetime.md)配合[Calendar](/root.js/calendar.md)和[Clock](/root.js/clock.md)大量更新。
* 修复[root.js基本库](/root.js/root.md)的一些问题。
* 修复[Animation动画库](/root.js/animation.md)的一些问题。

## v0.6.3

* [Coder](/root.js/coder.md) 编码器加入，封装了`CodeMirror`，实现PQL及其他语言的彩色编码。
* [TreeView树形目录组件](/root.js/treeview.md)的`TreeNode`节点类新增`expandOnRender`属性，用于确定首次加载时是否自动展开节点。