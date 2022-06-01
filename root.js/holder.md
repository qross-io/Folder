# 数据占位符

数据占位符可以理解为是一种显示数据的规则，这些数据从后端获取后需要呈现在页面上，数据占位符会在呈现时被替换为对应的数据。数据占位符在 root.js 标签库中被广泛应用到各种组件和标签，可以说与数据相关的标签或组件都会用到。

这些组件包括但不限于：

* Model 标签
* FOR 标签
* SPAN 标签
* O 标签
* TEMPLATE 标签
* SELECT 标签
* CHART 标签
* TABLE 标签
* TREEVIEW 标签

在每个标签相关的文档中都会介绍数据占位符的规则和用法，本文对所有的规则进行一下汇总。

在 Javascript 中，数据可以分为 5 种类型，单值、对象、单值数组、对象数组和复合类型（一般为对象）。

* 单值，如字符串、数字、布尔值等。
* 对象，例如可以用 Json 表示为
    ```json
    {
        "name": "Tom",
        "age", 18
    }
    ```
* 单值数组，可以用 Json 表示为
    ```json
    [1, 2, 3, 4, 5]
    ```
    或
    ```json
    ["MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"]
    ```
* 对象数组，可以理解为是一个二维表格
    ```json
    [
        {
            "name": "Tom",
            "age", 18
        },
        {
            "name": "Jerry",
            "age", 17
        }
    ]
    ```
* 复合结构，以上任意结构的组合。
    ```json
    {
        "timestamp": "2021-08-05 17:37:25",
        "message": "success",
        "data": [
            {
                "name": "Tom",
                "age", 18
            },
            ...
        ]
    }
    ```

从后端获取的数据多种多样，数据占位符就是用来加载和呈现这些不同结构的数据。数据占位符本质上是一些符号规则，例如`@stat.data`。一个包含多个规则的占位符是这样的`@modelName.propertyName[index].method()?(defaultValue)`，下面是关于每个符号的解释：

* 数据占位符总是以`@`开头。
* `modelName`指向某个 MODEL 元素的名字`name`或`id`，或者一个数据的名字，依据所属标签的不同而不同。
* 使用点规则`.`获取下一个子属性。
* 使用中括号`[n]`获取数组索引，如`[0]`。如果`n`为负数，表示倒数，如`-1`表示倒数第一个，依次类推。
* 也可以使用中括号`[attr]`获取子属性，如`[propertyName]`。特别时属性名中包含非变量字符（英文字母、数字和下划线）时，如中文，必须使用这种方式。
* 使用`|column|`获取对象数组的列，如`|studentName|`。
* 使用`.method()`调用方法，如`.avg()`，支持常量参数，如`.substring(0,4)`。字符串参数不需要写引号，布尔类型和数字类型参数类型会自动识别。
* 使用`?(defaultValue)`设置默认值，即占位符前面的数据为`null`或`undefined`时给一个默认值。
* 在占位符结尾使用叹号`!`来避免字符冲突。
* `data`是保留字，一般指当前标签的数据，如`@data.first`。

下面举例说明每一项的作用。 

```html
<model name="students" data="select name, age from students order by age asc">
    <set $="#Youngest">The youngest students is: @data[0][name]!. He/She is only @data[0].age!!</set>
    <set $="#Oldest">The oldest students is: @data[-1].name!. He/She is only @data[-1][age]!!</set>
    <set $="#Avg" value="~{ @students|age|!.avg().round(2) }"></set>
</model>
<span id="Youngest"></span>
<span id="Oldest"></span>
<input id="Avg" type="text" />
<for var="student" in="@students">
    <div>@student.name: @student[age]</div>
</for>
```

这个 MODEL 元素返回一个对象数组，也就是一个二维表格。各个占位符的逻辑说明如下：

* 在 MODEL 内部，使用`@data`指向当前 MODEL 元素获取的整个数据；在 MODEL 外部使用可以写为是`@students`。
* `@data[0][name]`表示第一行数据`name`项，与`@data[0].name`等价，`[0]`表示数组的第一项，当前代表返回数据的第一行。
* 同理，`@data[0].age`与`@data[0][age]`也等价，如果占位符以英文字母结尾，后面如果跟着有可能冲突的字符如`.`、`!`或英文字母等，则需要加一个叹号`!`表示占位符的结尾，叹号`!`可避免识别错误。
* `@data[-1].name`中的`[-1]`表示数组最后一项，倒数第二项为`[-2]`，依次类推。
* `@students|age|!`表示取数据的`age`列，得到一个数组。因为后面跟着一个小数点`.`，所以要加一个叹号`!`表示占位符结尾。例中使用了一个 Javascript 对结果数组进行了再加工，详见 [Express 字符串](/root.js/express.md)。其中`avg()`和`round(2)`是数组和数字的扩展方法，属于 Javascript 的内容。当数据加工过程比较复杂时一般会用到 Javascript 短句或表达式，这里可以全部使用占位符，可以写成`value="@data|age|.avg().round(2)"`或`value="@data.avg(age).round(2)"`。
* 使用 FOR 标签遍历整个结果数据，`in`属性中使用`@students`调取的是整个 MODEL 元素的值。这里不能使用`@data`，因为已经出了 MODEL 元素的范围。
* FOR 标签声明了`student`来表示数据的每一行，使用`@student.name`可以得到这一行中`name`项的值。`@student[age]`和`@student.age`等价。
* 在文本内容中输出`@`的问题，如果冲突时可以使用`&#64;`代替，例如电子邮件地址中有`@`字符。


---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [SELECT 标签扩展](/root.js/select.md)
* [Chart 图表组件](/root.js/chart.md)
* [TreeView 树形目录](/root.js/treeview.md)
* [Express 字符串](/root.js/express.md)
