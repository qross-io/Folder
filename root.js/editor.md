# 文本编辑 Editor

**Editor 元素扩展** 为页面上可显示内容的标签如`SPAN`、`TD`、`DIV`等提供原地或弹出框内的文本编辑功能，并在编辑完成后可以提交给后端。编辑模式根据输入类型不同有多种模式可选，如文本、数字、选择框等。

Editor 组件的历史比较久远了，前后一共经历了四个大版本。

1. Editor 的第一个版本诞生于 2006 年，当前记得是应用于博客的用户自定义设置，为博客用户提供前后台一致所见即所得的设置功能。还记得当时的名字叫 TextEditor。
2. 第二个版本开发于 2010 年，当前为数据管理后台提供表格单元格的数据编辑功能，名字叫 TdEditor。
3. 第三个版本开发于 2018 年，为了 Keeper 管理后台完全重新开发，名字改为 Editor，提供了超过 10 种编辑功能，后来这些功能慢慢移出来成为独立组件或其他组件的一部分。在`retired`目录可以找到旧版本。
4. 现在这是第 4 个版本，精简代码并丰富功能，也转变开发思路，与整个框架统一。

## 使用

使用需要以下相关文件。

* `root.js` 基础库
* `root.editor.js` 编辑功能实现。
* `root.popup.js` 在弹出框中进行编辑时使用，如果没有引用则使用浏览器默认的弹出框。
* `root.input.js` 使用 [INPUT 扩展](/root.js/input.md)需要的功能时使用。

## 属性

Editor 不是独立的组件，可以理解为是为其他标签提供的附加功能。

```html
<span editable="yes">HELLO</span>
```

使用`editable`属性为标签提供编辑功能，可使用[服务器端事件](/root.js/server.md)`onchange+`与后端交互。

```html
<span id="Title" editable="yes" onchange+="UPDATE comments SET title='{value}' WHERE id=#{id}">My Title</span>
```

上例中`{value}`表示编辑后的值，也可以使用`{text}`。`#{id}`是使用 [Voyager](/voyager/query.md) 处理的地址参数，如果没有使用 Voyager，可以使用客户端地址参数`&{id}`。

其他可用的属性如下：

* `type` 编辑器类型，支持 INPUT 标签的各种输入类型如`text`、`textarea`、`integer`、`number`等，原生下拉选择框`select`和多行输入框`textarea`。因为编辑器是附加到其他元素之上的，所有有可能其他元素也可能有`type`属性，这时可以使用属性名`editor-type`来避免冲突。
* `allow-empty`或`allowEmpty` 是否允许空值，默认为`false`。
* `kilo` 在数字编辑模式下是否使用千分符`,`，如`1000000`会显示成`1,000,000`。
* `placeholder` 在空值时的显示文字，意义同 INPUT 输入框的`placeholder`属性。
* `edit-in-prompt` 在弹出框中进行编辑，默认`false`。
* `prompt-text` 设置了这个属性会在弹出窗口中编辑值，这是显示在输入框上面的提示文字。
* `prompt-title` 弹出窗口的标题文字。

相关按钮及样式的属性如下：

* `edit-button-style` 在要编辑的文字后面显示的编辑按钮的形式，可选不显示`none`、按钮`button`和文字`text`，默认值为`text`。
* `edit-button-icon` 设置编辑按钮的图标，支持 Iconfont 和图片，默认为 Iconfont 值 `icon-edit`。
* `edit-button-text` 设置编辑按钮的文字，默认值为空。
* `edit-button-class` 设置编辑按钮的样式，默认值为空。
* `confirm-button-style` 在编辑时显示在输入框后面的确认按钮的形式，可选不显示`none`、按钮`button`和文字`text`，默认值为`button`。弹框编辑模式下始终为按钮。
* `confirm-button-icon` 设置确定按钮的图标，支持 Iconfont 和图片，默认值为空。
* `confirm-button-text` 设置确定按钮的文字，默认值为`OK`。弹出窗口模式下设置弹出窗口中的确认按钮的文字。
* `confirm-button-class` 确认按钮的样式，默认值为`small-compact-button blue-button`。
* `cancel-button-style` 在编辑时显示在输入框后面的取消按钮的形式，可选不显示`none`、按钮`button`和文字`text`，默认值为`text`。弹框编辑模式下始终为按钮。
* `cancel-button-icon` 设置取消按钮的图标，支持 Iconfont 和图片，默认值空。
* `cancel-button-text` 设置取消按钮的文字，默认值为`Cancel`。弹出窗口模式下设置弹出窗口中的取消按钮的文字。
* `cancel-button-class` 取消按钮的样式，默认值为`editor-link-button`。

因为编辑时需要使用输入控件，除了`type`类型外，还提供了额外的属性来设置控件。控件有三种类型，INPUT、TEXTAREA 和 SELECT。设置控件的属性就在要编辑的元素上添加以控件类型开头的属性即可。例如`input-autosize="yes"`、`textarea-rows`等。[INPUT 输入框](/root.js/input.md)可以使用各种扩展属性，TEXTAREA 和 SELECT 可以使用原生属性。

## 开始编辑

如果显示了文字右侧的编辑按钮（默认显示为一个编辑图标），则点击按钮开始编辑，另一个启用编辑的方式是单击要编辑的文字。如果想改变开始编辑的方式，可以在元素上添加`edit-on`属性，例如`edit-on="dblclick"`，这样就改成了双击开始编辑。当然也可以通过点击其他元素开始编辑，如`edit-on="click: #Button1"`。

## 事件

* `onedit` 开始编辑时触发，支持`return`。
* `oncancel` 取消编辑时触发，支持`return`。
* `onchange` 当文本值修改时触发更新，支持`return`。对应的[服务器端事件](/root.js/server.md)`onchange+`，用来向后端提交修改。


---
参考链接

* [root.js 基础库](/root.js/root.md)
* [INPUT 输入框扩展](/root.js/input.md)
* [Popup 弹出框组件](/root.js/popup.md)