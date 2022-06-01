# FOR 标签

FOR 标签也是 [Model 数据加载模型](/root.js/model.md)的一部分，引用文件与 Model 相同。FOR 标签提供了在 HTML 页面中循环显示 HTML 代码的能力，一般放在 BODY 中需要的位置。**FOR 标签只执行一次，呈现内容后会删除自己**。

```html
<for var="var" in="pql|url|array|object|m to n" container="selector" onload="event">
    ...html code
</for>
```

各个属性分别说明如下：

* `var`或`let`，功能类似于 MODEL 标签的`name`，可以用于调取`in`中的数据，在嵌套循环中强烈建议使用，否则可能会发生数据混乱，其他情况下可忽略。
* `in` 属于[`data`属性](/root.js/data.md)，但支持数字区间，如`0 to 9`。详见下面的说明。在属性中如果遇到双引号冲突时，可以用`&quot;`代替
* `container` 展示数据的容器，一般不需要设置。在特殊情况下，如要在 SELECT 标签中列表 OPTION，FOR 标签不会被浏览器识别，需要在 SELECT 标签之外设置 FOR。见文章后面的说明。
* `onload` 事件，加载完成后执行。事件只能写在标签之上。

## 数字区间

数字区间的格式是`m to n`，其中`m`和`n`表示整数数字，`m`可以比`n`小。可使用`item`保留字获取每个数字，也可以使用`var`或`let`属性声明自己的变量名。

```html
<for in="1 to 10">
    <span>@item</span>
</for>
或
<for var="i" in="8 to 1">
    <span>@i</span>
</for>

<for var="num" in="[1, 2, 3, 4, 5]">
    <span>@num</span>
</for>
```

## 遍历单值数组

* 建议声明`var`或`let`属性，否则需要使用`item`保留字。
* 当不声明`var`或`let`属性时，如果单个项是 Object，可以使用`@item.key`调取数据。
* 占位符语法规则 `@var`，其中`var`代表变量名; 在不声明`var`属性时，使用`@item`调取属性的值。末尾使用`!`防止字符冲突。

```html
<for in="select name FROM students -> FIRST COLUMN">
    <span>@item</span>
</for>

<for let="student" in="select name FROM students -> FIRST COLUMN">
    <span>Hello, @student!!</span> <!--只会输出 1 个叹号-->
</for>
```

## 遍历对象

* 建议声明`var`属性，如`let="k,v"`。否则默认使用`key`和`value`保留字，即通过`@key`得到对象每项的键, 使用 `@value`得到对象每项的值。如果只声明一个变量，则第二个使用默认保留字。
* 占位符语法规则 `@key`, `@value!`, `@value.path`, `@value[1]`等。

调取键的方法：

* 在声明变量名时，使用`@var1`或`@var1!`，其中`var1`代表变量名; 
* 在不声明 key 变量名时，使用`@key`或`@key!`, 其中`key`为保留字;
* 对象键属性值不支持默认值设置。

调取值的方法：

* 在声明变量名时, 使用`@var2`或`@var2!`, 其中var2为变量名。
* 在不声明 value 变量名时，使用`@value`，其中`value`为保留字。
* 嵌套结构中，有需要进一步获取 value 中下层某项的值。在声明变量名时，使用`@var.path`或`@var[1]`等，在不声明变量名时，使用`@value.path`

```html
<for in="/api/student">
    <span>@key: @value</span>
</for>

<for let="col" in="/api/student">
    <span>@col: @value</span>
</for>

<for let="k, v" in="/api/student">
    <span>@k: @v</span>
</for>

<for var="name,age" in='{ "name": "Tom", "age": 5, "score": }'>
    <div>@name - @age</div>
</for>
```

完整的占位符规则参阅[数据占位符](/root.js/holder.md)。

## 遍历对象数组

* 在不声明 var 属性时，使用`@item.name`可得到数组中某一个对象的一项指定的值。
* 在声明 var 属性时，使用 `@var1.name`，其中`var1`为 var 属性的名字。

```html
<!--下面 3 个示例效果完全相同-->
<for in="select name, age from students">
    <div>@item.name!! @item.age!. </div>
</for>

<for in="select name, age from students">
    <div>@item.name! @item[age]. </div>
</for>

<for var="student" in="select name, age from students">
    <div>@student.name!! @student.age!. </div> <!--只会输出 1 个叹号-->
</for>

<!--调取 Model 中的数据-->
<for let="child" in="@children.data">
    <div>@child.name - @child.age</div>
</for>
```

FOR 标签占位符也支持默认值，即`?(defaultValue)`

```html
<for var="field, value" in="select * from students -> first row">
    <span>@field: @value?(N/A)</span>
</for>
```

完整的占位符规则参阅[数据占位符](/root.js/holder.md)。

## 特殊情况

SELECT 标签的 OPTION 标签和 TABLE 标签的 TR 标签不支持 FOR 循环，即不能这样做

```html
<select>
    <for in="...">
        <option>data...</option>
    </for>
</select>
```
或
```html
<table>
    <for in="...">
        <tr>
            <td>data...</td>
        </tr>
    </for>
</table>
```

这种情况下如果是 SELECT 可以通过设置`container`属性并把 FOR 标签放到 SELECT 外面，如

```html
<select id="Fav"></select>
<for in="..." container="#Fav">
    <option>data...</option>
</for>
```

也可以使用 TEMPLATE 标签代替 FOR 标签。

```html
<select>
    <template data="...">
        <option>data...</option>
    </template>
</select>
```

[SELECT 标签](/root.js/select.md)和[TABLE 标签](/root.js/table.md)现在也已支持 [`data`属性](/root.js/data.md)，可以加载并呈现数据。

```html
<select data="...">...</select>
<table data="...">...</table>
```

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [SELECT 标签扩展](/root.js/select.md)
* [TABLE 标签扩展](/root.js/table.md)
* [与数据相关的属性](/root.js/data.md)
* [数据占位符](/root.js/holder.md)