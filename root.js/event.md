# 事件表达式

为了简化页面制作，进一步消除页面上的 Javascript 代码，组件库提供了 **事件表达式**。

```html
<span data="..." reload-on="click:#Button1">...</span>
<input update-value-on="change:#Select1,#Select2" value:="$(#Select1).$(#Select2)" />
```

上例中，`reload-on`属性的值`click:#Button1`即为事件表达式，表示当按钮`#Button1`执行事件`click`时，即触发 SPAN 标签的`reload`操作。其中冒号`:`前面是事件名，冒号`:`后面是触发事件的元素标签，同选择器格式。翻译为 Javascript 为：

```javascript
$x('#Button1').on('click', function() {
    span.reload();
});
```

事件名和选择器都支持多个，使用逗号分隔。更多示例：

* `onchange:@SelectButton1,#Select2` 事件名的`on`可以省略，`@SelectButton1`表示选择名为`SelectButton1`的自定义扩展标签。当`SelectButton1`和`Select1`触发`onchange`事件时都可执行。
* `focus,blur:input[type=text]` 当页面上所有 INPUT 输入框的`focus`和`blur`事件触发时执行。

事件表达式的应用暂时比较少，会慢慢扩展到更多标签上。

```html
<select id="Select1" data="...">
<select id="Select2" data="..." reload-on="change:#Select1">
<select id="Select3" data="..." reload-on="change:#Select1,#Select2">
```

上例实现了一个 SELECT 扩展标签的三级联动。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [data 扩展属性](/root.js/data.md)
