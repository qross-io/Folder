# 布尔属性扩展

在 HTML 中，有一些布尔类型的属性，如`selected`和`disabled`等。一般情况下这样使用

```html
<select disabled>
    <option selected="selected"></option>
</select>
```

如上例，布尔属性可以赋值也可以不赋值，都会生效。问题是，即使这样我们给属性赋相反的值`disabled="false"`，也一样会禁用组件。标签库对布尔属性进行了扩展，让属性值能够按照预期的方式进行工作，并对属性进行了增强。主要逻辑如下：

* 如果有[数据占位符](/root.js/holder.md)，则[从 Model 中获取数据](/root.js/model.md)并替换相应的占位符。
* 进行 [Express 字符串](/root.js/express.md)运算。
* 值`true`、`yes`、`1`、`ok`、`on`、`selected`、`disabled`、`enabled`、`visible`、`hidden`或`空字符串`，会识别为`true`，不区大小写。其中的`selected`、`disabled`、`enabled`、`visible`和`hidden`分别对应相应的属性值赋值习惯，如`disabled="disabled"`，空字符串也是因为这几个属性可以赋空值时也表示`true`。
* 值`null`、`undefined`、`false`、`no`、`0`、`cancel`、`off`，会识别为`false`，不区分大小写。
* 如果是表达式，如`3 > 2`，则尝试进行计算，表达式中支持`this`关键字指向当前元素标签。
* 其他值尝试使用`eval()`方法进行转换，计算失败在控制台返回异常信息并返回默认值；
* 默认值一般为`true`，根据场景不同有时会改变默认值。

也就是说，布尔属性除了支持单值以外，还支持逻辑表达式运算。

```html
<if test="$(#Count) < 2">...</if>
<div if="@score > 90">...</div>
<log termial="@amount == 0"></log>
<table meet="'@status' == 'success'">...</table>
```

由此可见，因为布尔属性支持[数据占位符](/root.js/holder.md)和 [Express 字符串](/root.js/express.md)，所有布尔属性本身就是一种[增强属性](/root.js/plus.md)，只不过不需要在属性名称后面写加号`+`。注意在属性中占位符只是用来恢复数据，不会判断数据类型，如果是字符串记得加引号。

布尔属性的应用范围仅限于标签库内的标签，也就说所有标签库内标签的布尔值类型的属性都支持以上逻辑。

布尔属性在标签中赋值时可以使用以上的逻辑，但是在 Javascript 中使用时需要用布尔值赋值，例如：`input.disabled = false`。就是说这些属性只用上面的逻辑解析一次，但也有多次判断的属性，例如`terminal`、`meet`等。这类多次判断的属性可以再次赋值为字符串，但一般用不到。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [数据占位符](/root.js/holder.md)
* [Exprss 字符串](/root.js/express.md)