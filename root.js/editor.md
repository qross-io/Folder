# 文本编辑 Editor

**Editor 元素扩展** 为页面上可显示内容的标签如`SPAN`、`TD`、`DIV`等提供原地或弹出框编辑功能，有多种模式可选，如文本、数字、切换按钮等。

## 版本历史

Editor 组件的历史比较久远了，前后一共经历了四个大版本。

1. Editor 的第一个版本诞生于 2006 年，当前记得是应用于博客的用户自定义设置，为博客用户提供前后台一致所见即所得的设置功能。还记得当时的名字叫 TextEditor。
2. 第二个版本开发于 2010 年，当前为数据管理后台提供表格单元格的数据编辑功能，名字叫 TdEditor。
3. 第三个版本开发于 2018 年，为了 Keeper 管理后台完全重新开发，名字改为 Editor，提供了超过 10 种编辑功能，后来这些功能慢慢移出来成为独立组件或其他组件的一部分。在`retired`目录可以找到旧版本。
4. 现在这是第 4 个版本，精简代码并丰富功能，也转变开发思路，与整个框架统一。

## 属性列表

Editor 不是独立的组件，可以理解为是为其他标签提供的附加功能。

```html
<span editable="yes">HELLO</span>
```

使用`editable`属性为标签提供编辑功能，可使用[服务器端事件](/root.js/server.md)`onchange+`与后端交互。

```html
<span id="Title" editable="yes" onchange+="UPDATE comments SET title='{value}' WHERE id=#{id}">My Title</span>
```

上例中`{value}`表示编辑后的值，也可以使用`{text}`。`#{id}`是使用 [Voyager 处理的地址参数](/voyager/query.md)，如果没有使用 Voyager，可以使用客户端地址参数`&{id}`。

其他可用的属性如下：

* `input-type` 输入框类型，支持`text`、`textarea`、`integer`、`decimal`、`percent`和`select`。
* `allow-empty` 是否允许空格，默认为`false`。
* `kilo` 在数字编辑模式下是否使用千分符`,`，如`1000000`会显示成`1,000,000`。
* `placeholder` 在空值时的显示文字，意义同 INPUT 输入框的`placeholder`属性。
* `allowempty` 是否允许空值。

其他内容待完善。


---
参考链接

* [INPUT 输入框扩展](/root.js/input.js)
