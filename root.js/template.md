# TEMPLATE 标签

TEMPLATE 标签也是 [Model 数据加载模型](/root.js/model.md)的一部分，引用文件与 Model 相同。**TEMPLATE** 标签可以理解为局部的 MODEL 标签，同时支持重新加载。TEMPLATE 不支持再嵌套 TEMPLATE 标签，但 TEMPLATE 里面可以嵌套 [FOR 标签](/root.js/for.md)和 [IF 标签](/root.js/if.md)。TEMPLATE 更不能嵌套 MODEL 标签, 也没有任何必要。

TEMPLATE 标签也可应用到其他自定义组件中，如 [TreeView 标签](/root.js/treeview.md)和 [TABLE 标签扩展](/root.js/table.md)。

## TEMPLATE 属性

```html
<template name="name" var="variable name" data="url|array|object|pql" path="jsonPath" as="array|list|for|loop|collection|object" page="int" increment="primary key" offset="0" auto-refresh="yes|no" interval="ms" terminal="boolean expression">
    html code...
</template>
```

* `name` 名称或 ID。
* `as` 将数据做为列表还是对象，对应值分别是`array`和`object`，`array`别名还有`list`、`for`、`loop`、`collection`，`object`为默认值。此属性为一次声明，不支持事后修改。
* `var`或`let` 当作为列表或者循环使用时，指定循环的变量名，就和 FOR 标签一样。
* `page` 按页码进行分页时的初始值，一般为`0`或`1`，默认为`0`，分页时自动更新，可以通过`$:page`占位符传递给`data`属性。
* `increment` 按主键进行分页时需要通过这个属性设置主键名，跟返回的数据里的名称相同。
* `offset` 按主键进行分页时主键的起始值，可以通过`$:offset`占位符传递给data属性。
* `lazy-load` [布尔属性](/root.js/boolean.md)，是否滚动增量加载，可选`yes`、`no`、`1`、`0`、`true`、`false`等布尔值或表达式。
* `clear-on-refresh`或`clear-on-reload` 是否在重新加载数据前先清空原有数据。
* `auto-refresh` [布尔属性](/root.js/boolean.md)，是否支持自动更新。
* `interval` 自动更新的间隔时间，单位“毫秒”，默认`2`秒。
* `terminal` [布尔属性](/root.js/boolean.md)，自动更新的终止条件，可使用`@:`得到当前 TEMPLATE 数据。

占位符语法规则请参见[数据占位符](/root.js/holder.md)。当不设置`as`属性时，保留字`data`表示当前 TEMPLATE 加载的数据。当`as`设置为`array`时，保留字`item`表示本次循环的数据。当`as`设置为`object`时，保留字`key`和`value`表示本次循环的数据。`item`、`key`和`value`可以由属性`var`进行设置替换。

## TEMPLATE 方法

TEMPLATE 与其他标签不同的是提供了方法，如：

```javascript
$('#name').setData(data).asArray().clear().load(func);
```

可用方法有：

* `setData(data)` 如果要加载和第一次不同的数据，则通过这个方法再次指定。
* `asArray()` `asList()` `asLoop()` 功能同 `as="array"` 属性。
* `setContainer(container)` 设定呈现 HTML 的容器元素。
* `setPosition(position)` 设置 HTML 在容器中呈现的位置，可选项有：`befeoreEnd` 容器的结束标签之前，这是默认值；`afterEnd` 容器的结束标签之后；`beforeBegin` 容器的开始标签之前；`afterBegin`容器的开始标签之后。
* `setPage(page)` 手工设置页码, 按页码分页时使用。
* `setOffset(offset)` 设置分页偏移量，按主键分页时使用。
* `clear()` 在重新加载内容时先清空。
* `load(func)` 将内容添加到页面上, 设置`func`可以指定在添加完成后执行的函数，独立于`onload`事件，即如果同时设置了`onload`并传递了`func`函数，都会执行。
    ```javascript
    $('#name').setData(data).load(func);
    ```

## TEMPLATE 事件

* `onload` 第一次加载完成时触发。
* `onreload` 每次重新加载完成时触发，不包括第一次。
* `onlazyload` 每次增量加载完成后触发，不包括第一次。
* `onterminate` 满足自动更新的终止条件时触发。
* `ondone` 增量加载所有数据完成之后触发，在`onterminate`事件之后大约 3 次`interval`时间。

```javascript
$('#name').on('load', function(data) {
    // data 是返回的数据集
}).on('reload', function(data) {
    // ...
});
```

第一种方式无段等待页面加载完成，仅用于事件绑定。第二种方式需要在 TEMPLATE 组件加载完成之后调用。

## 简单示例

```html
<tempalte name="students" data="/api/students">
    <div>Scores of all Students</div>
    <for in="@data">
        <div>@item.name: @item.score</div>
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

其中`@:data`指向返回结果的`data`属性项，`@item.name`和`@item.score`指向每数据行的`name`和`score`属性项。

## As Array

作为列表使用时，需要在标签上增加`as`属性或使用`asList()`方法。在解析时程序会自动为内容增加`<for in="@data"> ... </for>`包围，上一节的示例可以修改为：

```html
<div>Scores of all Students</div>
<tempalte name="students" as="array" data="/api/students -> data">
    <div>@item.name: @item.score</div>
</tempate>
```


## 数据懒加载

TEMPLATE 标签可以很方便的实现数据懒加载，支持移动端。TEMPLATE 标签支持两种懒加载方式：

```html
<template as="array" page="1" lazy-load="yes" data="/api/list?page=$:[page]">
    <div>@item.title</div>
    <diV>@item.description</div>
</template>

<template var="project" as="array" lazy-load="yes" data="select title, description from projects limit ~{ $:[page] * 100 }, 100">
    <div>@project.title</div>
    <diV>@project.description</div>
</template>
```

上面两个示例中，实现逻辑一致，其中`data`属性取数据的方式不同。`as`属性`loop`和`list`效果相同，它们都是`array`的别名。`$:[page]`用来获取当前页码，如果不显式设置`page`属性，则默认值为`0`。

```html
<template var="project" as="array" lazy-load="yes" increment="id" offset="15434" data="select id, title, description from projects where id>$:[offset] limit 100">
    <div>@project.title</div>
    <diV>@project.description</div>
</template>
```

上例为另一种懒加载方式，需要指定`increment`属性，属性名对应返回结果集的主键字段名，系统会自动记录`offset`，即每次查询主键的最大值，并在下次查询时自动使用新值。`offset`如果不设置则默认为`0`。

## 自动刷新

TEMPLATE 标签还支持自动刷新操作，见下例：

```html
    <template var="task" name="abc" as="array" auto-refresh="yes" interval="2000" clear-on-refresh="no" data="select id, task_time from qross_tasks limit ~{ Math.randomNext(2, 10) }">
        <div class="f24">@task.id</div>
        <diV class="space">@task.task_time</div>
    </template>
```

上例中`auto-refresh`必须设置为`true`或其他可用真值或表达式的结果为`true`。`interval`指自动刷新的间隔时间，单位“毫秒”，默认值为`2`秒。`clear-on-refresh`设置为真值表示每次都清空，如果不清空则表示增加增加，默认为`true`。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [FOR 循环遍历](/root.js/for.md)
* [Express 字符串](/root.js/express.md)
* [数据占位符](/root.js/holder.md)