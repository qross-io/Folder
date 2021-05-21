
# Model 数据加载模型

## 简介

**Model** 组件提供多种非常简单的方式在页面上显示数据源（Api 接口或 PQL 语句）中的数据，并可以直接执行 PQL 语句（需要配置后端 Controller）。Model 组件通过自定义标签或属性的方式加载数据，支持循环和分支，目的是减少 Javascript 代码的编写。Model 还为其他组件提供数据加载支持。

Model 相关标签的执行都在客户端，即页面呈现给用户之后再加载页面上需要的数据。

## 引用

```html
<script type="text/javascript" src="@/root.js"></script>
<script type="text/javascript" src="@/root.model.js"></script>
```

## MODEL 标签

MODEL 标签一般放到 HEAD 标签中，用于从数据源调取数据并展示在页面相应的元素中。MODEL 标签主要用于一次加载在多个地方显示数据的场景，支持自动刷新。

```html
<model name="name" data="url|array|object|pql" auto-refresh="yes|no" terminal="boolean expression" interval="integer">
    <set $="selector" attr="name" if="boolean"></set>
</model>
```

上面的 SET 标签可以根据需要是否选用，见下面的说明。

* `name` MODEL 的名字，在页面上获取 MODEL 中的数据时会用到
* `data` 支持 URL 地址、PQL、数组和对象，直接输入数组或对象时需要能正确转成 json 对象，详见[data属性](/root.js/data.md)
* `auto-refresh` 是否自动刷新
* `terminal` 自动刷新时的结束条件，支持表达式，如`@:data.length == 0`，即当返回的数据为空时结束自动刷新。`@:data.length`表示返回数据的`data`键下的数组的长度；可用`@:/`表示所有数据，这种情况下`/`不可省略。其中`@:`的意义是指向当前数据，冒号不可省略。
* `interval` 自动刷新的间隔时间，单位秒，可以为小数。

### 数据呈现方式一

第一种方式是 **通过 SET 子标签更新页面上元素的数据**。建议使用这种方式对数据进行更新，不仅与页面耦合低，且支持自动刷新。SET 子标签属性说明如下：

* `$` selector 选择器
* `attr` 要更新的属性名, 不设置即表示`innerHTML`或`value`属性。
* `if` 更新的条件，js 支持表达式

示例：

```html
<model name="students" data="select grade, count(0) as amount from students -> first row" auto-refresh="yes" interval="2" terminal="@:amount >= 100">
    <set $="#Grade"></set>
    <set $="#Amount" if="@:amount > 10"></set>
</model>

<span id="Grade">@:grade</span>
<span id="Amount">@students.amount</span>
```

上例中，每`2`秒钟刷新一次标签`#Amount`的值，当`amount`值达到`100`时，停止刷新。

### 数据呈现方式二

第二种方法是 *通过设置标签属性调取 MODEL 中的数据*。目前支持在任意元素的`innerHTML`和部分元素属性中调取 MODEL 中的数据，支持如下：

* 任意元素的`innerHTML`中，需要在元素中添加`-html`属性，可以为任意值
* A 元素的`href`属性，需要在`href`属性名前加中横线 `-href`
* INPUT 元素的`value`属性，同样也要在前面加中横线 `-value`
* 任意元素的`title`属性，同样也要在属性前面加中横线`-title`
* FOR 标签的`in`属性，见下面的介绍
* IF 或 ELSIF 标签的`test`属性，见下面的介绍
* TEMPLATE 标签的`data`属性，见下面的介绍

示例：

```html
<model name="students" data="select grade, count(0) as amount from students -> first row">

<span -html>@students.grade</span>
<if test="@students.amount > 100">
    <input -value="@students.amount" />
</if>
```

### 调取 MODEL 中的数据的占位符规则

数据占位符格式为 `@modelName.property[index]?(defaultValue)`

* `@` 必须有
* `modelName` 指定`Model`的数据。
* 使用`.`规则访问属性，如`@school.data`。
* 使用方括号`[`和`]`访问数组，如`@students[0]`。
* 点规则和方括号规则可以互换，如`@school[data]`或`@students.0`。
* 支持多层引用，如`@students[0].age`。
* 结尾使用叹号`!`防止字符冲突，如`-value="@student.name!andOtherWords"`，如果不加叹号会导致识别错误
* `?(defaultValue)` 为可选项，当前面的值为`null`时，设置默认值。
* 在 SET 标签中，可以使用`:`表示当前 MODEL，如`@:students[0]`或`@:[0].name`等。

示例：

```html
<model name="children" data="/api/children"></model>
```

假如 Model Name 为`children`，数据内容为

```json
{
    "message": "success",
    "elapsed": 324,
    "data": [
        {
            "name": "Tom",
            "age": 5
        },
        {
            "name": "Jerry",
            "age": 3
        },
        ......
    ]
}
```

* 调取整个 MODEL 的数据 - `@children`
* 调取`message` - `@children.message`
* 调取第一个孩子的名字 - `@children.data[0].name`
* 如果不存在`elapsed`属性，能默认为`0` - `@children.elapsed?(0)`
* 如果默认值为空字符串，则写为 `@childern.message?()`
* 不建议直接在`modelName`上使用默认值，如`@children?(empty)`

更多示例：

```html
<input -value="@children.data[1].age]" />
<span -html>@children.data[0].name</span>
<for in="@childre.data"></for>
```

补充：程序会将占位符替换为实际的数据，如果没找到且没有设置默认值，此位置仍保留占位符文本，以利于查错或避免文本冲突（如电子邮件地址）。

### Model 事件

MODEL 标签可用的事件如下：

* `onload` 第一次加载完成后触发，仅执行一次
* `onrefresh` 每次刷新完成后触发，第一次加载不算刷新

调用示例：

```javascript
$listen('name').on('load,refresh', function(data) {
    //每次加载都执行
});

$model('name').on('refresh', function(data) {
    //仅刷新后执行
});
```

事件也可以直接写在标签上：

```html
<model name="test" data="/api/test" onload="tell()"></model>
```

注：`$listen`和`$model`的区别是前者只用于绑定事件，后者可以操作 Model 对象，且只能在加载完成后使用。对于 MODEL 标签来说，因为没有公开的方法，所以`$model`只在程序内部使用。上例中`data`参数是 MODEL 加载的数据。

## O 标签

MODEL 标签可以一次查询然后将数据显示在不同的标签（元素）的属性中，经常我们查询只使用一次，所以框架提供了 **O 标签**用于实现一次性数据查询需求。O 标签在 **加载完成后自动移除**，只保留加载的文本数据。

```html
<o>SELECT title FROM table WHERE id=#{id} -> FIRST CELL</o>
```

也可以使用 [Voayger 模板引擎](/voyager/overview.md) 的服务器端代码实现。

```sql
<%=SELECT title FROM table WHERE id=#{id} -> FIRST CELL%>
```

不同点是前者是在客户端实现的异步查询，在页面呈现给用户之后执行；后者是在服务器端实现的查询，是在页面呈现给用户之前。O 标签与[`data`属性](/root.js/data.md)实现的逻辑完全相同，也支持接口或跨域接口。

```html
<o>/api/page/title?id=#{id} -> /data</o>
<o>SELECT title FROM table1 WHERE id=$id -> FIRST CELL</o>
```

## SPAN 标签扩展

O 标签不是原生标签，不是样式控制及更多功能，为了实现更灵活的数据定制功能，组件库扩展了 SPAN 标签，也可用于输出数据。

```html
<span onload="event" onreload="event" reload-on="click:#Button1" data="SELECT title, views FROM table1 WHERE id=$id -> FIRST ROW">文章“@:title”的阅读数是 @:views!!</span>
```

* `onload` 第一次数据加载完成后触发的事件。
* `onreload` 重新加载数据后触发的事件。
* `reload-on` 指定重新加载数据的条件，由其他组件触发。规则详情[事件表达式](/root.js/event.md)。

SPAN 标签的事件只能写在标签上，SPAN 扩展标签不提供扩展标签的选择器。SPAN 标签可以使用其他原生属性来做其他操作，比如控制样式。

SPAN 标签的占位符语法规则为 `@:keyOrPath?(defaultValue)`

上例中，`data`属性返回一个有两个字段的对象，如可以是`{ "title": "低代码平台简单介绍", "views": 209 }`。`@:title`可以获取`低代码平台简单介绍`，`@:views`可以获取到`209`，最后 SPAN 标题中展示`文章“低代码平台简单介绍”的阅读数是 209!`。`@:views`后面有两个叹号，前一叹号表示占位符结尾，防止字符冲突。可使用`@:/`获取`data`属性的所有数据。

## FOR 标签

FOR 标签提供了在 HTML 页面中循环显示 HTML 代码的能力，一般放在 BODY 中需要的位置。注意：**FOR 标签只执行一次，呈现内容后会删除自己**。

```html
<for var="var" in="pql|url|array|object|m to n" container="selector" onload="event">
    ...html code
</for>
```

各个属性分别说明如下：

* `var`或`let`，功能类似于 MODEL 标签的`name`，可以在调取`in`中的数据时使用，在嵌套循环中强烈建议使用，否则可能会发生数据混乱，其他情况下可忽略。在遍历对象时，可以同时声明两个变量名，如`let="k,v"`。如果只声明一个，则第二个使用默认保留字。 `key`和`value`是保留字，即可以通过`@key`来调取键数据，通过`@value`调取值数据。在遍历单值数组时，`item`是保留字，可以通过`@item`调取单值数据。使用叹号`!`结尾以防止字符冲突。
* `in`类似于[`data`属性](/root.js/data.md)，但支持数字区间，如`0 to 9`。详见下面的说明。在属性中如果遇到双引号冲突时，可以用`&quot;`代替
* `container` 展示数据的容器，一般不需要设置。在特殊情况下，如要在 SELECT 标签中列表 OPTION，FOR 标签不会被浏览器识别，需要在 SELECT 标签之外设置 FOR。
* `onload` 事件，加载完成后执行。事件只能写在标签之上。

### 数字区间

数字区间的格式是`m to n`，其中`m`和`n`表示整数数字。可使用`item`保留字获取每个数字，也可以使用`var`或`let`属性声明自己的变量名。

```html
<for in="1 to 10">
    <span>@item</span>
</for>
或
<for var="i" in="1 to 10">
    <span>@i</span>
</for>

<for var="num" in="[1, 2, 3, 4, 5]">
    <span>@num</span>
</for>
```

### 遍历单值数组

* 建议声明`var`或`let`属性，否则需要使用`item`保留字。
* 当不声明`var`或`let`属性时，如果单个项是 Object，可以使用`@[key]`调取数据。
* 占位符语法规则 `@var`，其中`var`代表变量名; 在不声明`var`属性时，使用`@item`调取属性的值。末尾使用`!`防止字符冲突。

```html
<for in="select name FROM students -> FIRST COLUMN">
    <span>@item</span>
</for>

<for let="student" in="select name FROM students -> FIRST COLUMN">
    <span>Hello, @student!!</span> <!--只会输出 1 个叹号-->
</for>
```

### 遍历对象

* 建议声明`var`属性，否则可使用`key`和`value`保留字，即通过`@key`得到对象每项的键, 使用 `@value`得到对象每项的值。
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

### 遍历对象数组

* 在不声明 var 属性时，使用`@[name]`可得到数组中某一个对象的一项指定的值。
* 在声明 var 属性时，使用 `@var1.name`，其中`var1`为 var 属性的名字。

```html
<!--下面 3 个示例效果完全相同-->
<for in="select name, age from students">
    <div>@item.name!! @item.age!. </div>
</for>

<for in="select name, age from students">
    <div>@[name]! @[age]. </div>
</for>

<for var="student" in="select name, age from students">
    <div>@student.name!! @student.age!. </div> <!--只会输出 1 个叹号-->
</for>

<!--下面 2 个调取 Model 中的数据-->
<for in="@children.data">
    <div>@[name] - @[age]</div>
</for>

<for var="child" in="@children.data">
    <div>@child.name! - @child.age!</div>
</for>
```

FOR 标签占位符也支持默认值，即`?(defaultValue)`

```html
<for var="field, value" in="select * from students -> first row">
    <span>@field: @value?(N/A)</span>
</for>
```

### 特殊情况

SELECT 标签的 OPTION 标签和 TABLE 标签的 TR 标签不支持循环，即不能这样做
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

也可以使用 TEMPLATE 标签代替 FOR 标签。TABLE 标签现在可以使用 [DATATABLE 组件](/root.js/datatable.md)来遍历数据源的数据，[SELECT 标签](/root.js/select.md)现在也已支持 [`data`属性](/root.js/data.md)。


## IF 标签

IF 标签用于逻辑判断，逻辑成立时才显示标签中的内容。注意：**IF 标签只执行一次，呈现内容后会删除自己**。

```html
<if test="boolean expression" onload="event" onreturntrue="event" onreturnfalse="event">
    ...html code
<elsif test="boolean expression">
    ...html code
<else>
    ...html code
</if>
```

* `test` 必须有，没有则默认结果为`false`。这本质是一个 Javascript 表达式，接受 MODEL、FOR、[地址参数和 DOM 占位符](/root.js/express.md)。
* `elsif` 标签可以有多个，**注意拼写**。
* `else` 标签只能有一个，如果设置了多个，则只有第一个生效。
* `onload` 事件，加载完成后执行。这个事件只能写在标签上。
* `onreturntrue` 所有 IF 或 ELSIF 条件有一条成功时触发。
* `onreturnfalse` 所有 IF 或 ELSIF 条件都失败时触发。

```html
<for in="select * from students">
    @[name]: 
    <if test="@[score] > 90">
        Excellent!
    <elsif test="@[score] > 75">
        Good Job!
    <elsif test="@[score] > 60">
        Keep studing.
    <else>
        Are you OK?
    </if>
</for>

<if test="$(#A) != ''">
    ...
<elsif test="&(A) != null">
    ...
<elsif test="">
    ...
</if>
```

### 特殊情况下的 IF 处理

SELECT 标签的 OPTION 标签和 TABLE 标签的 TR 标签不仅不支持 FOR 标签作为它们的父级，也不支持 IF 标签，这种情况下可以通过设置`if`属性来确定是否呈现，如：

```html
<table>
    <template as="list">
        <tr if="@[age] >= 18">
            ...
        </tr>
    </template>
</table>
```

补充说明:

* FOR 和 IF 语句块的 HTML 代码中支持 FOR、地址参数和 DOM 占位符，不支持 MODEL 的占位符。
* 如果非要向 FOR 和 IF 语句块中传递 MODEL 的值，可使用 FOR 标签的`in`属性向下传递。另一种方法是在 FOR 和 IF 的语句块中使用 MODEL 标签支持的扩展属性。


## TEMPLATE 标签

**TEMPLATE** 标签可以理解为局部的 MODEL 标签，同时支持重新加载。TEMPLATE 不支持再嵌套 TEMPLATE 标签，但 TEMPLATE 里面可以嵌套 FOR 和 IF。TEMPLATE 更不能嵌套 MODEL 标签, 也没有任何必要。TEMPLATE 标签比 MODEL 支持的功能更多，比如懒加载、自动刷新等功能。

TEMPLATE 标签也可应用到其他自定义组件中，如 [TreeView 标签](/root.js/treeview.md)和 [DataTable 标签](/root.js/datatable.md)。

### TEMPLATE 属性

```html
<template name="name" var="variable name" data="url|array|object" path="jsonPath" as="array|list|for|loop|collection|object" page="int" increment="primary key" offset="0" auto-refresh="yes|no" interval="second" terminal="boolean expression">
    html code...
</template>
```

* `name` 名称或 ID
* `as` 将数据做为列表还是对象，对应值分别是`array`和`object`，`array`别名还有`list`、`for`、`loop`、`collection`，`object`为默认值。此属性为一次声明，不支持事后修改。
* `var`或`let` 当作为列表或者循环使用时，指定循环的变量名，就和 FOR 标签一样。
* `page` 按页码进行分页时的初始值，一般为`0`或`1`，默认为`0`，分页时自动更新，可以通过`$:page`占位符传递给`data`属性
* `increment` 按主键进行分页时需要通过这个属性设置主键名，跟返回的数据里的名称相同
* `offset` 按主键进行分页时主键的起始值，可以通过`$:offset`占位符传递给data属性
* `lazy-load` 是否滚动增量加载，可选`yes`、`no`、`1`、`0`、`true`、`false`等布尔值
* `clear-on-refresh` 是否在重新加载数据前先清空原有数据
* `auto-refresh` 是否支持自动更新
* `interval` 自动更新的间隔时间，单位“秒”，支持小数，默认`2`秒。
* `terminal` 自动更新的终止条件，可使用`@:`得到当前数据。

占位符语法规则为 `@:keyOrPath?(defaultValue)`

与 MODEL 不同的是, MODEL 必须声明`name`属性, 并需要使用`name`来调取值; TEMPLATE 在调取数据时不使用`name`，而使用`:`，表示当前TEMPLATE，如`@:data[0].name`。使用`@:/`调取TEMPLATE 的所有数据。

### TEMPLATE 方法

TEMPLATE 与其他标签不同的是提供了方法，如：

```javascript
$tempalte('name').load(data, path).asArray().clear().append(func);
```

可用方法有：

* `$template` 为固定选择器方法，如`$template('name')`，需要在页面加载完成之后才能使用，见下面的`$finish`全局方法。
* `load(data)` 如果要加载和第一次不同的数据，则通过这个方法再次指定。
* `asArray()` `asList()` `asLoop()` 功能同 `as="array"` 属性。
* `setContainer(container)` 设定呈现 HTML 的容器元素。
* `setPosition(position)` 设置 HTML 在容器中呈现的位置，可选项有：`befeoreEnd` 容器的结束标签之前，这是默认值；`afterEnd` 容器的结束标签之后；`beforeBegin` 容器的开始标签之前；`afterBegin`容器的开始标签之后。
* `setPage(page)` 手工设置页码, 按页码分页时使用。
* `setOffset(offset)` 设置分页偏移量，按主键分页时使用。
* `clear()` 在重新加载内容时先清空。
* `append(func)` 将内容添加到页面上, 设置`func`可以指定在添加完成后执行的函数，独立于`onload`事件，即如果同时设置了`onload`并传递了`func`函数，都会执行。
    ```javascript
    $template('name').load(data).append(func);
    ```

### TEMPLATE 事件

* `onload` 第一次加载完成时触发。
* `onlazyload` 每次增量加载完成时触发，不包括第一次。
* `onterminate` 满足自动更新的终止条件时触发。
* `ondone` 增量加载所有数据完成之后触发，在`onterminate`事件之后大约 3 次`interval`时间。

```javascript
$listen('name').on('load', function(data) {
    // data 是返回的数据集
})

$template('name').on('lazyload', function(data) {
    // ...
})

$t('name').ondone = function() {
    //...
}
```

第一种方式无段等待页面加载完成，仅用于事件绑定。第二和第种方式需要在 TEMPLATE 组件加载完成之后调用。`$template`只能选择 TEMPLATE 标签，`$t`可以选择任意自定义标签。

### 简单示例

```html
<tempalte name="students" data="/api/students">
    <div>Scores of all Students</div>
    <for in="@:data">
        <div>@[name]: @[score]</div>
    </for>    
</tempate>
```

假如上例接口返回结果为:

```json
{
    "data": [
        {
            "name": "Tom",
            "score": 78
        },
        {
            "name": "Jerry",
            "score": 85
        },
        ...
    ],
    "message": "ok"
}
```

其中`@:data`定位到返回结果的`data`属性项，`@[name]`和`@[score]`定位到每数据行的`name`和`score`属性项。

### As Array / List

作为列表使用时，需要在标签上增加`as`属性或使用`asList()`方法。在解析时程序会自动为内容增加`<for in="@:/"> ... </for>`包围，循环体内部必须使用与 FOR 标签相同的占位符语法。上一节的示例可以修改为：

```html
<div>Scores of all Students</div>
<tempalte name="students" as="list" data="/api/students -> data">
    <div>@[name]: @[score]</div>
</tempate>
```

### 数据懒加载

TEMPLATE 标签可以很方便的实现数据懒加载，支持移动端。TEMPLATE 标签支持两种懒加载方式：

```html
<template as="loop" page="1" lazy-load="yes" data="/api/list?page=$:[page]">
    <div>@[title]</div>
    <diV>@[description]</div>
</template>

<template as="list" lazy-load="yes" data="select title, description from projects limit ~{ $:[page] * 100 }, 100">
    <div>@[title]</div>
    <diV>@[description]</div>
</template>
```

上面两个示例中，实现逻辑一致，其中`data`属性取数据的方式不同。`as`属性`loop`和`list`效果相同，它们都是`array`的别名。`$:[page]`用来获取当前页码，如果不显式设置`page`属性，则默认值为`0`。

```html
<template as="list" lazy-load="yes" increment="id" offset="15434" data="select id, title, description from projects where id>$:[offset] limit 100">
    <div>@[title]</div>
    <diV>@[description]</div>
</template>
```

上例为另一种懒加载方式，需要指定`increment`属性，属性名对应返回结果集的主键字段名，系统会自动记录`offset`，即每次查询主键的最大值，并在下次查询时自动使用新值。`offset`如果不设置则默认为`0`。

### 自动刷新

TEMPLATE 标签还支持自动刷新操作，见下例：

```html
    <template name="abc" as="loop" auto-refresh="yes" interval="2" clear-on-refresh="no" data="select id, task_time from qross_tasks limit ~{ $random(2, 10) }" interval="2">
        <div class="f24">@[id]</div>
        <diV class="space">@[task_time]</div>
    </template>
```

上例中`auto-refresh`必须设置为`true`或其他可用真值。`interval`指自动刷新的间隔时间，单位“秒”，默认值为`2`秒。`clear-on-refresh`设置为真值表示每次都清空，如果不清空则表示增加增加，默认为真值。

## 全局事件

与全局方法`$ready`类似，在页面加载完成之后执行函数。区别是`$ready`文档准备好时执行，而`$finish`是在所有 MODEL 相关的标签解析完成之后再执行给定的函数。`$finish`和`$ready`一样，可使用多次。在执行时按函数的添加顺序执行。

```javascript
$finish(function() {
    //...
});
```

特别注意：

* 在所有模板标签加载完成之后触发！
* 各标签的加载顺序：MODEL -> TEMPLATE -> FOR 或 IF -> $finish() -> O -> SPAN
* 所有 MODEL 标签并行加载，所有 MODEL 加载完成之后，FOR 和 IF 按标签在文档出现中的位置顺序加载，谁在前谁先加载。

## 对嵌入数据进行再加工

有些时候，数据源返回的数据不一定能满足前端展示要求，如保留小数点两位，生成百分比，截取字符串等。这时候需要对返回的数据进行再加工，root 组件库提供了使用 Javascript 再次编辑数据的方法 ———— **Javascript 表达式占位符**。Javascript 表达式占位符可以理解为在 HTML 中写 Javascript，分为两种：

**Javascript 短句占位符** 格式为 `~{ javascript expression... }`

短句类似于赋值语句的右半部分，可以对数据进行再处理。如：

```html
<for in="...">
    <span>~{ '@[name]'.substring(0, 1) }</span>
</for>

<template>
    <span>~{ '$(#Amount)'.$p().toInt() }</span>
    <input type="text" value="~{ '&(money)'.$p().toPercent() }" />
</template>
```

极端情况下，业务逻辑有可能更复杂，短句也不能满足需求了，还可以使用

**Javascript 语句占位符**，格式为 `~{{ javascript statement... }}`

语句类似于一个函数内的所有语句，可以对数据进行任何操作，但必须有返回值。如：
```html
<if test="true">
    <div>
        ~{{
            if (@[age] > 18) {
                return 'adult';
            }
            else if (@[age] < 12) {
                return 'children';
            }
            else {
                return 'youth';
            }
        }}
    </div>
</if>
```

注意：`@` 符号在 Javascript 占位符表达式中为特殊字符，在必须要输出`@`字符且遇到冲突时，请使用`~u0040`代替。


---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Express 字符串](/root.js/express.md)
* [data 扩展属性](/root.js/data.md)
* [Voyager 模板引擎](/voyager/overview.md)