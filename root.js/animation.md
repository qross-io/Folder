# Animation 动画支持

这个支持库不是一个标签，也不算一个组件，是为其他标签和组件提供动画功能。动画支持库可以通过一条语句设置为元素实现动画功能。动画有三种表现形式，移动、缩放和透明度，但不支持旋转，三种表现形式可以混合使用。

支持库内由三部分组成，一部分是基础动画功能，Animation 对象及相应功能；第二部分与选择器配合的动画方法；第三部分为 Message 消息组件，用于在页面上部显示不同类型的提示信息。

这个支持库的历史也比较久远了，记得是 HTML 5 刚刚出现时写的，应该是 2010 年左右，代码里还有用 Visual Studio 写的注释。作者在 2000 年左右的时候做过一段时间的 Flash 动画制作，所以这个库的设计也参考了 Flash 的一些思想。

## 动画设置

设置方法过于复杂，作者正在思考简化的方法。

## 动画方法

动画方法需要配合`$x`选择器使用，可以理解为是基础库的扩展，例如：

```javascript
$('#Button1').fadeInt();
$('#Link1').fadeOut();
$('#Div').slideInt();
```

包含 4 个基础方法：

* `fadeIn(opacity = 0, duration = 0.6)` 淡入，`opacity`是初始透明度，值范围`0`到`1`之间的小数，`duration`是持续时间，单位是秒。
* `fadeOut(opacity = 0, duration = 0.6)` 淡出，`opacity`是结束透明度，值范围`0`到`1`之间的小数，`duration`是持续时间，单位是秒。
* `move(distanceX = 0, distanceY = 0, duration = 0.6)` 让元素移动一段距离。`distanceX`和`distanceY`可以为负数。
* `resetAnimation()`移除元素上的动画效果。
* `slideIn(distanceX = 0, distanceY = 0, duration = 0.6)` 滑入。`distanceX`和`distanceY`表示滑动的开始位置，`distanceX`支持设置为`w`和`-w`，表示元素的宽度，`distanceY`支持设置为`h`和`-h`，表示元素的高度。`distanceX`最大距离限制`500`，`distanceY`最大距离限制`400`，`duration`最大限制`1.25`秒。
* `slideOut(distanceX = 0, distanceY = 0, duration = 0.6, hidden = false)` 滑出。`distanceX`和`distanceY`表示滑动的结束位置，`hidden`表示在动画结束时是否隐藏元素。其他同`slideIn`方法。

## Message 组件

消息组件使用时在页面上方显示指定的提示文字，支持不同的颜色和背景，用于不同的目的，比如报错等场景，比 alert 更友好一些。消息组件一般配合其他组件使用，如 TABLE、BUTTON 等。

```javascript
Message.red('出错啦！').show(5);
```

上例的`red`方法即指定相应的颜色，其他选项包括`green`、`blue`、`orange`、`yellow`、`orange`共 6 种。`show`方法参数是显示的时间，单位是秒，不设置或设置为`0`表示一直显示。鼠标点击消息会隐藏，和 Callout 一样。

另外，还有一个`hideLast()`方法用于隐藏上一条还在显示的 Message，保证只显示一条 Message。

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Button 按钮扩展](/root.js/button.md)
* [Popup 弹出框](/root.js/popup.md)
* [TABLE 表格扩展](/root.js/table.md)