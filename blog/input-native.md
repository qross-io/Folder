# INPUT 标签参考

INPUT 标签是最常用的表单组件，用来接收用户输入的信息。

```html
<input id="TextBox1" type="text" />
```

可用的`type`类型有：

* `button` 按钮，建议用 BUTTON 标签代替
* `checkbox` 复选框
* `color` 颜色选择器
* `date` 日历
* `datetime` 目前无效果
* `datetime-local` 日期和时间选择，不包含秒。
* `email` 电子邮件
* `file` 选择文件，用于文件上传
* `hidden` 隐藏字段
* `image` 图像提交按钮
* `month` 月选择框
* `number` 数字，只能输入数字、小数点、正负号和`e`（科学计数法）
* `password` 密码
* `radio` 单选框
* `range` 范围`0 ~ 100`
* `reset` 重置表单按钮
* `search` 搜索文本框
* `submit` 提交按钮
* `tel` 电话号码
* `text` 文本框
* `time` 时间选择，不包含秒。
* `url` URL 地址
* `week` 周选择框

其他可用的原生属性有：

* `accept` 值 `audio/* video/* image/* MIME_type` 规定通过文件上传来提交的文件的类型，只适用于`file`类型
* `alt` 定义图像输入的替代文本，只适用于`image`类型。
* `autocomplete` 可选择`on`、`off`，输入字段是否应该启用自动完成功能。
* `autofocus` 当页面加载时 INPUT 元素应该自动获得焦点。
* `checked` 属性规定在页面加载时应该被预先选定，只适用于`checkbox`或者`radio`类型。
* `disabled` 是否禁用的元素。
* `form` 规定 INPUT 元素所属的一个或多个表单，值为表单的 id。
* `formaction` 属性规定当表单提交时处理输入控件的文件的 URL，只适用于`submit`和`image`类型。
* `formenctype` 可选值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`，属性规定当表单数据提交到服务器时如何编码，只适用于`submit`和`image`类型。
* `formmethod` 可选`get`、`post`等，定义发送表单数据到 action URL 的 HTTP 方法，只适用于`submit`和`image`类型。
* `formnovalidate` 属性覆盖 FORM 元素的`novalidate`属性。
* `formtarget` 可选`_blank`、`_self`、`_parent`、`_top`或 framename，规定表示提交表单后在哪里显示接收到响应的名称或关键词，只适用于`submit`和`image`类型。
* `height` 规定 INPUT 元素的高度，单位像素，只适用于`image`类型。
* `list` 引用 DATALIST 元素，其中包含 INPUT 元素的预定义选项，值为 DATALIST 元素的 id。
* `max` 属性规定 INPUT 元素的最大值，适用于`number`和`date`类型。
* `maxlength` 属性规定 INPUT 元素中允许的最大字符数。
* `min` 属性规定 INPUT 元素的最小值，适用于`number`和`date`类型。
* `multiple` 属性规定允许用户输入到 INPUT 元素的多个值。
* `name` 属性定义元素的名称。
* `pattern` 属性规定用于验证 INPUT 元素的值的正则表达式字符串。
* `placeholder` 属性规定可描述输入 INPUT 字段预期值的简短的提示信息。
* `readonly` 属性规定输入字段是只读的。
* `required` 属性规定必需在提交表单之前填写输入字段。
* `size` 属性规定以字符数计的 INPUT 元素的可见宽度。
* `src` 属性规定显示为提交按钮的图像的 URL，只适用于`image`类型。
* `step` 属性规定 元素的数字间隔，适用于`number`类型。
* `value` 获取或指定INPUT 的值。
* `width` 属性规定 INPUT 元素的宽度，只适用于`image`类型。 

---
参考链接

* [INPUT 标签扩展](/root.js/input.md)
* [HTML 通用属性](/blog/html.md)