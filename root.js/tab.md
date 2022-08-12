# TAB 标签视图

在前端开发中，经常需要在一组元素中控制选中或通过选中某一项显示和隐藏其他元素，TAB 标签视图用来实现多元素中选中一项比如导航选择、标签选择、菜单项选择等功能。

与其他组件不同，TAB 不是一个标签，而是一个属性，这个属性定义在其他的标签之上，可以是 DIV、TABLE 等。

TAB 属性从原有的 FocusView 组件重构而来，FocusView 组件第一版编写于 2005 年，用来实现类似的功能，不仅支持焦点视图，还支持多选、鼠标事件等功能。目前 TAB 已经是第五版，简化功能的同时增加了与当前 root.js 框架互动的支持。

TAB 的使用非常简单，只有几个属性和事件。

```html
<table id="Languages" cellpadding="10" cellspacing="20" border="0" tab bindings="TD" default-class="language" selected-class="language-focus"
         onchange="$cookie.set('language', this.value)"
         onchange+="put:/api/personal/language?language={value}" success-text="{text}" exception-text="Exception: {error}">
    <tr>
        <td selected="<%= IF @cookies.language == 'chinese' THEN 'yes' ELSE 'no' END %>" value="chinese" text="语言设置已更改，刷新页面将会切换到中文界面。" to-show="#ChineseCurrent" to-hide="#EnglishCurrent">简体中文<br/><span id="ChineseCurrent">(当前语言)</span></td>
        <td selected="<%= IF @cookies.language == 'english' THEN 'yes' ELSE 'no' END %>" value="english" text="Your setting is changed. Please refresh page to enable it." to-show="#EnglishCurrent" to-hide="#ChineseCurrent">English<br/><span id="EnglishCurrent">(Current Language)</span></td>
    </tr>
</table>
```

以上为 Master 项目中设置语言的逻辑，这里以此为例说明一下 TAB 组件的属性和事件。

* `tab` 关键属性，必须有，表示这个元素支持标签”。一般设置在 TABLE、DIV 等父级元素上。属性值任意或不设置。
* `bindings` 选项的选择器，决定了这个标签有多少个选项。上例绑定到`TD`上，即只能两个选项。
* `excludings` 当`bindings`的选择结果中有不想绑定的元素时，可以使用这个属性排除，可以设置索引序号（从`0`开始），也支持奇数`odd`、偶数`even`、最后一个`-1`、倒数倒数第二个`-2`等，多个值之间使用逗号`,`分隔。
* `defaultClass` 选项在非选择状态下的样式。
* `selectedClass` 选项在选中状态下的样式。
* `ontabchange` 客户端事件，当选项改变时触发。上例用于将新值保存到 Cookie 中。
* `ontabchange+` [服务器端事件](/root.js/server.md)，当选项改变后触发。上例用于请求后端接口。
* `successText` 服务器端执行符合预期后的提醒文字。
* `failureText` 服务器端执行符合不预期后的提醒文字。
* `exceptionText` 服务器端执行出错时的提醒文字。
* `message`或`messageDuration`，使用 Message 方式显示提醒文字，值为 Message 的显示时间，`0`为一直显示。需要引入动画组件`root.animation.js`。
* `callout`或`calloutPosition`，使用 Callout 方式显示提醒文字，值为 Callout 的显示位置，可选`up`、`down`、`left`和`right`。
* `value` 被选中项的值，需要先在选项上进行设置。
* `text` 被选中项的文本，需要先在选项上进行设置。


每个标签选项的属性有：

* `value` 当前选项的值，可以用来保存一些需要的数据。
* `text` 当前选项的文本，可以用来保存一些需要的数据。
* `to-hide` 当选中这个选项时，要隐藏的元素，值为 CSS 选项器。
* `to-show` 当选中这个选项时，要显示的元素，值为 CSS 选项器。`hide`比`show`先执行。
* `selected` 是否默认选中，属性值为可识别为布尔值的值或表达式。上例使用了服务器端代码来判断 Cookie 中保存的值。

在支持 Expres 字符串的属性中增强属性上，有三个可用的变量：

* `{value}` 表示标签当前选中项的值，在选项元素上定义。如上例中`value="chinese"`和`value="english"`，`language={value}`中的`{value}`指向这个属性。
* `{text}` 表示标签当前选中项的文本文字，在选项元素上定义。如上例中`text="Your setting is changed. Please refresh page to enable it."`，`success-text="{text}"`中的`{text}`指向这个属性。
* `{data}` 这个不是属性，表示请求后端返回的结果，`success`和`failure`状态下为接口返回接口，`exception`状态下为异常文字。上例中`exception-text="Exception: {data}"`中的`{data}`指向这个属性，这里的`data`也可以用`error`代替。

TAB 组件与 SELECT 逻辑有些像，区别在于 SELECT 会生成生成相关的子元素，但 TAB 只是绑定到现有的元素上。

TAB 本质上是一个扩展属性，所属的类名为 HTMLTabAttribute，通过应用元素的`tab`属性访问这个类的实例，如`$('#Languages').tab`，但一般用不到。


---
参考链接

* [SELECT 选择列表组件](/root.js/select.md)
* [服务器端事件](/root.js/server.md)
* [Express 字符串](/root.js/express.md)