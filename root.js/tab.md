# TAB 标签视图

在前端开发中，经常需要在一组元素中控制选中或通过选中某一项显示和隐藏其他元素，TAB 组件即用来实现多元素中选中一项比如导航选择、标签选择、菜单项选择等功能。

TAB 组件从原有的 FocusView 组件重构而来，FocusView 组件第一版编写于 2005 年，用来实现类似的功能，不仅支持焦点视图，还支持多选、鼠标事件等功能。目前 TAB 已经是第四版，简化功能的同时增加了与当前 root.js 框架互动的支持。

TAB 的使用非常简单，只有简单的几个属性和事件。

```html
<table id="Languages" cellpadding="10" cellspacing="20" border="0" tab="yes" bindings="TD" default-class="language" selected-class="language-focus"
         onchange="$cookie.set('language', this.value)"
         onchange+="put:/api/personal/language?language={value}" success-text="{text}" exception-text="{data}">
    <tr>
        <td selected="<%= IF @cookies.language == 'chinese' THEN 'yes' ELSE 'no' END %>" value="chinese" text="语言设置已更改，刷新页面将会切换到中文界面。" show="#ChineseCurrent" hide="#EnglishCurrent">简体中文<br/><span id="ChineseCurrent">(当前语言)</span></td>
        <td selected="<%= IF @cookies.language == 'english' THEN 'yes' ELSE 'no' END %>" value="english" text="Your setting is changed. Please refresh page to enable it." show="#EnglishCurrent" hide="#ChineseCurrent">English<br/><span id="EnglishCurrent">(Current Language)</span></td>
    </tr>
</table>
```

以上为 Master 项目中设置语言的逻辑，这里以此为例说明一下 TAB 组件的属性和事件。

* `tab` 关键属性，必须有，表示将这个元素转成“标签”。一般设置在 TABLE、DIV 等父级元素上。属性值任意。
* `bindings` 选项的选择器，决定了这个标签有多少个选项。上例绑定到`TD`上，即只能两个选项。
* `excludes` 当`bindings`的选择结果中有不想绑定的元素时，可以使用这个属性排除，可以写索引序号（从`0`开始），也支持奇数`odd`、偶数`even`、最后一个`l`或`n`，或者 Javascript 表达式如`n-1`表示倒数第二个。
* `default-class` 选项在非选择状态下的样式。
* `selected-class` 选项在选中状态下的样式。
* `onchange` 客户端事件，当选项改变时触发。上例用于将新值保存到 Cookie 中。
* `onchange+` [服务器端事件](/root.js/server.md)，当选项改变后触发。上例用于请求后端接口。
* `success-text` 服务器端执行符合预期后的提醒文字，只有 Callout 一种提醒方式。
* `failure-text` 服务器端执行符合不预期后的提醒文字，只有 Callout 一种提醒方式。
* `exception-text` 服务器端执行出错时的提醒文字，只有 Callout 一种提醒方式。
* `value` 表示标签当前选中项的值，在选项元素上定义。如上例中`value="chinese"`和`value="english"`，`language={value}`中的`{value}`指向这个属性。
* `text` 表示标签当前选中项的文本文字，在选项元素上定义。如上例中`text="Your setting is changed. Please refresh page to enable it."`，`success-text="{text}"`中的`{text}`指向这个属性。
* `data` 表示请求后端返回的结果，`success`和`failure`状态下为接口返回接口，`exception`状态下为异常文字。上例中`exception-text="{data}"`中的`{data}`指向这个属性。

每个标签选项的属性有：

* `value` 当前选项的值。
* `text` 当前选项的文本。
* `hide` 当选中这个选项时，要隐藏的元素，值为 CSS 选项器。
* `show` 当选中这个选项时，要显示的元素，值为 CSS 选项器。`hide`比`show`先执行。
* `selected` 是否默认选中，属性值为可识别为布尔值的值。上例使用了服务器端代码来判断 Cookie 中保存的值。

TAB 组件与 SELECT 逻辑有些像，区别在于 SELECT 会生成生成相关的子元素，但 TAB 只是绑定到现有的元素上。TAB 将来有可能整合进 SELECT。


---
参考链接

* [SELECT 选择列表组件](/root.js/select.md)
* [服务器端事件](/root.js/server.md)
* [Express 字符串](/root.js/express.md)