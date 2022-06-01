# Model 数据加载模型

**Model** 组件提供多种非常简单的方式在页面上显示数据源（Api 接口或 PQL 语句）中的数据，并可以直接执行 PQL 语句（需要配置后端 Controller）。Model 组件通过自定义标签或属性的方式加载数据，目的是减少 Javascript 代码的编写。Model 还为其他组件提供数据加载支持。Model 模型中有多个标签组成，以实现不同的数据加载方式，主要有：MODEL 标签，用于从后端加载数据并为其他标签服务；[FOR 标签](/root.js/for.md)，实现循环；[IF 标签](/root.js/for.md)，实现条件判断和分支；[O 标签](/root.js/o.md)，用于一次性从后端加载数据；[SPAN 标签扩展](/root.js/span.md)，也可以从后端加载数据，支持重新加载；[TEMPLATE 标签](/root.js/template.md)，可以嵌入到其他标签中以提供数据加载支持，也可以独立使用。本文主要介绍 MODEL 标签，其他标签的说明见对应章节。

Model 模型中相关标签的执行都在客户端，即页面呈现给用户之后再加载页面上需要的数据。另外，Model 模型各标签在使用时会用到[数据占位符](/root.js/holder.md)，用于从模型或后端获取数据。

## 引用

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.model.js"></script>
```

## MODEL 标签

MODEL 标签一般放到 HEAD 标签中，用于从数据源调取数据并展示在页面相应的元素中。MODEL 标签主要用于一次加载在多个地方显示数据的场景，支持自动刷新。MODEL 标签主要用来显示对象类型的数据或单值数据，对于数组类型的数据，一般使用 FOR 或 TEMPLATE 加载。

```html
<model name="name" data="url|array|object|pql" auto-refresh="yes|no" terminal="boolean expression" interval="integer">
    <set $="selector" attr="name"></set>
</model>
```

上面的 SET 标签可以根据需要是否选用，见下面的说明。

* `name` MODEL 的名字，在页面上获取 MODEL 中的数据时会用到，必须设置。
* `data` 支持 URL 地址、PQL、数组和对象，直接输入数组或对象时需要能正确转成 json 对象，详见[data属性](/root.js/data.md)。
* `auto-refresh` [布尔属性](/root.js/boolean.md)，是否启用自动刷新。
* `terminal` 自动刷新时的结束条件，[布尔属性](/root.js/boolean.md)，支持表达式，如`@:data.length == 0`，即当返回的数据为空时结束自动刷新。`@:data.length`表示返回数据的`data`键下的数组的长度；可用`@:/`表示所有数据，这种情况下`/`不可省略。其中`@:`的意义是指向当前数据，冒号不可省略。
* `interval` 自动刷新的间隔时间，单位毫秒，默认值为`2000`，即`2`秒。

## 数据呈现方式一

第一种方式是 **通过 SET 子标签更新页面上元素的数据**。建议使用这种方式对数据进行更新，不仅与页面耦合低，且支持自动刷新。SET 子标签属性说明如下：

* `$` selector 单项选择器
* `attr` 要更新的属性名, 不设置即表示`innerHTML`或`value`属性。

示例：

```html
<model name="students" data="select grade, count(0) as amount, score from students -> first row" auto-refresh="yes" interval="2000" terminal="@data.amount >= 100">
    <set $="#Grade" value="@data.grade"></set>
    <set $="#Amount">@students.amount</set>
    <set $="#Score" value="@data.score" style:font-weight="~{ @data.score > 90 ? 'bold' : 'normal' }"></set>
</model>

<select id="Grade">...</select>
<span id="Amount"></span>
<input id="Score" size="10" />
```

上例中，相关逻辑如下：

* 每`2`秒钟刷新一次，当结果`amount`值达到`100`时，停止刷新。其中`@data`表示当前 Model 标签返回的结果。
* 每次刷新时，自动更新 3 个元素的数据，`#Grade`、`#Amount`和`#Score`。
* `#Grade`自动设置值为`@data.grade`，并且自动选中值对应的选项。
* 设置`#Amount`的`innerHTML`为`@students.amount`，`@students.amount`与`@data.amount`等价。
* 输入框`#Score`给两个属性赋值，一个是`value`；一个是样式的字体粗细，其中`style`表示主属性，后面的`font-weight`表示子属性，中间使用`:`分隔。设置的值除了支持数据占位符之外，还支持 [Express 字符串](/root.js/express.md)，这里的含义是当`@data.score`大于`90`时显示粗体，否则使用正常字体。

## 数据呈现方式二：增强属性

第一种方式是 MODEL 标签发起的数据更新，第二种方式是元素方发起的数据请求。语法规则是在属性名字后面多一个加号`+`，表示“增强”、“扩展”的意思。例如：

```html
<model name="students" data="select grade, count(0) as amount from students -> first row">

<span data>@students.grade</span>
<select value+="@students.grade">
    <option value="1">Grade 1</option>
    <option value="2">Grade 2</option>
    <option value="3">Grade 3</option>
</select>
<if test="@students.amount > 100">
    <input value+="@students.amount" />
</if>
```

有几个属性不需要在名称后面加加号`+`。

* FOR 标签的`in`属性，见下文 FOR 中的介绍
* IF 或 ELSIF 标签的`test`属性，见下文 IF 中的介绍
* 所有扩展标签和自定义标签的`data`属性。
* 所有的[布尔属性](/root.js/boolean.md)。

这个方式的优点支持元素的所有属性，使用限制是当前暂时仅支持自定义标签和已扩展的原生标签，不支持未扩展的原生标签。随着框架的不断完善，会支持的越来越多。第二种方式的加载顺序在第一种之后。请参阅[增强属性](/root.js/plus.md)查看更多介绍。

## 调取 MODEL 中的数据的占位符规则

数据占位符格式为 `@modelName.property|column|[index].method()?(defaultValue)!`。

* `@` 必须有，表示占位符开头。
* `modelName` 指定`Model`的数据。
* 使用`.`规则访问属性，如`@school.data`。
* 使用方括号`[index]`规则访问数组，如`@students[0]`。
* 使用`|column|`规则访问列，仅适用于对象数组，返回数组中每个对象指定字段的值，生成一个新的单元素数组，如`@students|amount|`。
* 点规则和方括号规则可以互换，如`@school[data]`或`@students.0`。
* 支持多层引用，如`@students[0].age`。
* 使用`.method()`调用数据类型的方法，支持常量参数，字符串不需要写引号，布尔和数字会自动识别。
* 结尾使用叹号`!`防止字符冲突，如`-value="@student.name!andOtherWords"`，如果不加叹号会导致识别错误
* `?(defaultValue)` 为可选项，当前面的值为`null`时，设置默认值。
* 在 SET 标签中，可以使用`data`表示当前 MODEL 的数据，如`@data.students[0]`或`@data[0].name`等。

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
* 调取所有孩子的名字 - `@children.data|name|`
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

程序会将占位符替换为实际的数据，如果没找到且没有设置默认值，此位置仍保留占位符文本，以利于查错或避免文本冲突（如电子邮件地址）。请参阅[数据占位符](/root.js/holder.md)查看所有规则及说明。

## 方法和事件

MODEL 标签可用的事件如下：

* `onload` 第一次加载完成后触发，仅执行一次。
* `onreload` 每次刷新完成后触发，第一次加载不算刷新。

因为`load()`方法是自动执行，所以可有的方法只有`reload()`，即在非自动更新时可手动或由其他组件触发更新 MODEL 的数据。

事件一般直接写在标签上：

```html
<model name="test" data="/api/test" onreload-="hide: #PopupLoading"></model>
```

也可以 Javascript 中调用：

```javascript
$('#name').on('load,reload', function(data) {
    //每次加载都执行
}).on('reload', function(data) {
    //仅刷新后执行
});
```

## Model 数据的双向绑定

Model 用于加载并为其他组件提供数据，主要用于一次加载且多个组件使用的场景。当 Model 的数据变化时（即执行了`reload`方法)，则可以自动重新加载使用数组的组件。当前支持 CHART、TABLE、SPAN、SELECT 和 TEMPLATE 标签。

```html
<div id="Range" value="1h" tab="yes" bindings="A" onchange-="reload: #Stat">...</div>
<model name="Stat" data="SELECT channel_name AS channelName, AVG(pageviews) AS pageviews FROM traffic WHERE range='$(#Range)'"></model>
<chart id="PageviewsChart" title="Channel PageViews" type="bar" x-axis="@Stat|channelName|" y-axis="@Stat|pageviews|"></chart>
```

上例中，名为`Stat`的 Model 用来从后端加载数据，接口参数依赖于标签`Range`的值，返回数据结构是一个包含列`channelName`和`pageviews`的二维表格；图表`PageviewsChart`从 Model 中加载数据，分别使用数据中的`channelName`列和`pageviews`列作为 X 轴和 Y 轴的数据；当 TAB 标签`Range`的选项切换时，触发`onchange`事件，让 Model 的数据根据`Range`的值重新加载；Model 加载完成后，图表`PageviewsChart`因为引用了 Model 的数据，也会自动被重新加载已展示新的数据。


## 全局事件

`Document`新增一个`onpost`事件，表示在 Model 及 Model 相关数据组件加载完数据之后触发，可绑定多个`onpost`事件。

```javascript
document.on('post', function() {
    //...
});
```

## 对嵌入数据进行再加工

有些时候，数据源返回的数据不一定能满足前端展示要求，如保留小数点两位，生成百分比，截取字符串等。这时候需要对返回的数据进行再加工，数据占位符虽然支持调用方法，但是当数据占位符也有搞不定的场景，root.js 标签库提供了使用 Javascript 再次编辑数据的方法 ———— **Javascript 表达式占位符**。Javascript 表达式占位符可以理解为在 HTML 中写 Javascript，分为两种：

**Javascript 短句占位符** 格式为 `~{ javascript expression... }`

短句类似于赋值语句的右半部分，可以对数据进行再处理。如：

```html
<for in="...">
    <span>{ '@item.name'.substring(0, 1) }</span>
</for>

<template>
    <span>{ '$(#Amount)'.$p().toInt() }</span>
    <input type="text" value="{ '&(money)'.$p().toPercent() }" />
</template>
```

极端情况下，业务逻辑有可能更复杂，短句也不能满足需求了，还可以使用

**Javascript 语句占位符**，格式为 `~{{ javascript statement... }}`

语句类似于一个函数内的所有语句，可以对数据进行任何操作，但必须有返回值。如：
```html
<if test="true">
    <div>
        {{
            if (@item.age > 18) {
                return 'adult';
            }
            else if (@item.age < 12) {
                return 'children';
            }
            else {
                return 'youth';
            }
        }}
    </div>
</if>
```

注意：`@` 符号在 Javascript 占位符表达式中为特殊字符，在必须要在文本内容中输出`@`字符且遇到冲突时，请使用`&#64;`代替。更多内容，见 [Express 字符串](/root.js/express.md)。

在 Javascript 短句中，有一个特殊的变量`data`，变量保存从后端取回来的数据。在 MODEL 标签中，表示整个数据。但在 [TEMPLATE 标签](/root.js/template.md)中的循环遍历模式下，还有 [FOR 标签](/root.js/for.md)中，`data`表示每一行的数据。

## 总结

Model 数据加载模型中涉及到好几个标签，现在总结一下各个标签的作用和区别。

* MODEL 标签可以从后端加载数据，然后更新其他任意标签的值或其他各种属性。自己本身不呈现数据。支持自动刷新和重新加载。
* [SPAN 标签](/root.js/span.md)可以将加载的数据呈现到自己的内容`innerHTML`中，支持重新加载。
* [O 标签](/root.js/o.md)也可以用来加载数据，但是在加载完成后移除自身。不支持重新加载。
* [IF 标签](/root.js/if.md)用来逻辑判断，不能从后端加载数据，用在页面上作判断，以确定该显示哪个分支的 HTML 内容。加载完成后移除自身，不支持重新加载。
* [FOR 标签](/root.js/for.md)用来循环显示集合数据，加载完成后同样移除自身，不支持重新加载。不建议嵌入到其他复合标签中，如 TABLE 标签、[SELECT 标签](/root.js/select.md)等。
* [TEMPLATE 标签](/root.js/template.md)可以理解为是 FOR 标签的加强版，支持自动刷新和重新加载，可以嵌入到其他复合标签中用于显示数据。如 [TABLE 标签](/root.js/table.md)、[TREEVIEW 标签](/root.js/treeview.md)等。
* IF 和 FOR 可以互相嵌套，TEMPLATE 不能被上面的标签嵌套，但可以嵌套 IF 和 FOR。

各个标签的加载顺序如下：**MODEL -> TEMPLATE -> FOR 或 IF -> document.onpost 事件 -> O -> SPAN**。其中，所有 MODEL 标签并行加载，FOR 和 IF 按标签在文档出现中的位置顺序加载，谁在前谁先加载。

各个标签除了可以从后端加载数据外，也支持各种静态数据。总之，使用 Model 模型中的各个标签，可以满足任意类型的数据加载需求。当然，其他有些标签也支持数据加载功能，如 SELECT 等，目的是丰富并简化与数据相关的开发。

---
参考链接

* [FOR 循环遍历](/root.js/for.md)
* [IF 逻辑判断](/root.js/if.md)
* [TEMPLATE 标签](/root.js/template.md)
* [O 数据输出](/root.js/o.md)
* [SPAN 标签扩展](/root.js/span.md)
* [数据占位符](/root.js/holder.md)
* [布尔属性](/root.js/boolean.md)
* [Express 字符串](/root.js/express.md)
* [root.js 基础库](/root.js/root.md)
* [data 扩展属性](/root.js/data.md)
* [Voyager 模板引擎](/voyager/overview.md)