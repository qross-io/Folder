# Select 组件扩展

**Select** 组件扩展了原生 SELECT 标签，增加了多种选择模式，并提供多选功能。

* 支持选择按钮、图片列表、单选框、筛选框等类型。
* 支持从后端加载数据。
* 支持多选。
* 支持通过服务器端事件与服务器端交互。
* 除新增属性外，对一些原生属性进行了扩展，如`text`、`value`等。
* 支持不同状态下的样式设置，并且支持单独设置选项的样式。

## 使用

Select 组件的相关依赖库和组件列表如下：

* `root.js` [基础库](/root.js/root.md)
* `root.model.js` [Model 数据加载模型](/root.js/model.md)
* `root.select.js` 主文件
* `/css/root/select.css` 默认样式表 

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.model.js"></script>
<script type="text/javascript" src="/root.select.js"></script>
<link href="/css/root/select.css" rel="stylesheet" type="text/css" />
```

<script type="text/javascript" src="@/root.select.js"></script>
<link href="@/css/root/select.css" rel="stylesheet" type="text/css" />

## 属性

像其他组件一样，Select 组件通过属性配置大部分功能。SELECT 标签下还可以包含一个或多个 OPTION 标签，下面分别介绍两个元素的属性。

```html
<select id="Select1" type="button"
     class="CSS" option-class="CSS" selected-option-class="CSS" disabled-option-class="CSS"
     multiple="boolean" data="api" onchange+="api">
    <option value="" title="" class="CSS" selected-class="CSS" disabled-class="CSS">text</option>
    ...
</select>
```

除支持所有原生属性外，SELECT 标签增加的扩展属性和事件如下：

* `await` 等待其他组件加载完成之后再加载数据，如`await="#A"`。当前 SELECT 需要使用其他组件的数据时，假如其他依赖的组件未加载完成，会产生错误，这个属性可以等待其依赖的组件加载完成再进行加载。
* `class` 整个框体的 CSS 样式，不同模式下框体的标签元素不同。
* `data` 从其他数据源生成 OPTION 列表，见本文下面的介绍。
* `disabled` 原生属性，是否禁用组件。组件库对这个属性进行了扩展，处理逻辑请参见[布尔类型的属性](/root.js/boolean.md)。
* `disabled-option-class` 禁用状态选项的 CSS 样式，根据不同的 type 默认值不同。
* `enabled` 是否启用组件，与`disabled`相对，但优先级低于`disabeld`。
* `multiple` 原生属性，是否支持多选，支持设置可识别为布尔值的所有字符串，如`yes`、`FALSE`、`1`等，默认`false`。
* `option-class` 每个选项的 CSS 样式，根据不同的 type 默认值不同。处理逻辑请参见[布尔类型的属性](/root.js/boolean.md)。
* `selected-option-class` 被选中选项的 CSS 样式，根据不同的 type 默认值不同。
* `text` 获取或设置 SELECT 的选中项的文本。
* `type` 选择框的模式，下面会分别介绍。如果没有这个属性则保持原始模式。
* `value` 获取或设置 SELECT 的选中项的值。通过设置这个属性可以设置默认选中的项，只要与对应项的值相同即可。优先级高于选项的`selected `属性。在`multiple`时可以设置逗号分隔的多个值以默认选中多项。

扩展事件有：

* `onchange` 可以理解为是原有的`onchange`事件，事件函数参数传递`index`，是目标选中项的索引。如`select.onchange = funcdtion(index, ev) { ...; return false; };`。
* `onchange+` 与`onchange`事件对应的[服务器端事件](/root.js/server.md)，支持接口和 PQL 语句。对应的状态事件是以下 4 个。
* `onchange+success` 当服务器端事件执行成功时触发的客户端事件。
* `onchange+failed` 当服务器端事件执行失败时触发的客户端事件。
* `onchange+exception` 当服务器端事件执行异常时触发的客户端事件。
* `onchange+completion` 当服务器端事件执行完成时触发的客户端事件。

和其他组件一样，有几个与[服务器端事件](/root.js/server.md)相关的属性：

* `success-text` 请求成功后的提示文字。
* `failure-text` 请示失败后的提示文字。
* `exception-text` 请求异常时的提示文字，一般指接口出错。
* `message-duration`或`message` 提示消息的持续时间，单位为“秒”，设置为`0`表示表示一直显示。
* `callout-position`或`callout` 弹出文字的位置，默认为`up`。

除支持所有原生属性外，OPTION 标签增加的扩展属性如下：

* `class` 选项的默认样式，不同模式下框体选项的标签元素不同。
* `disabled` 原生属性，是否禁用选项。组件库对这个属性进行了扩展，处理逻辑请参见[布尔类型的属性](/root.js/boolean.md)。
* `disabled-class` 选项在禁用状态时的样式。
* `enabled` 是否启用选项，与`disabled`相对，优先级低于`disabled`，处理逻辑请参见[布尔类型的属性](/root.js/boolean.md)。
* `hide` 当前选项被选中时，需要隐藏的元素，这里的值也是选择器，如`hide='span.hint'`。一般与`show`属性同时设置，`hide`先与`show`执行。
* `selected` 当前选项是否默认被选中。
* `selected-class` 选项在被选中时的样式。
* `show` 当前选项被选中时，需要显示的元素，这里输入选择器，如`show="#A,#B"`。
* `title` 在鼠标划过选项时的提示文字。

除了以上提到的属性外，不同的模式下还可能有自己的特有属性，将在各模式的说明里介绍。


## 按钮模式

按钮模式显示为一行单选或多选按钮，所有选项平铺展开。

```html
<select type="button" id="ButtonSelector">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>
```

显示效果如下：

<select type="button" id="ButtonSelector">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>

BUTTON 模式下 SELECT 标签的专有属性只有一个

* `scale` 用来设置按钮的大小，可选值有`mini`、`small`、`normal`、`big`、`large`，默认`normal`。当然也可以通过 option-class 设置按钮样式。

## 图片模式

图片模式可以使用图片作为选项。

```html
<select type="image" id="ImageSelector">
    <option value="mysql" src="/images/dbs/mysql.png">MySQL</option>
    <option selected value="mariadb" src="/images/dbs/mariadb.png">MariaDB</option>
    <option value="sqlserver" src="/images/dbs/sqlserver.png">SQL Server</option>
    <option value="postgresql" src="/images/dbs/postgresql.png">PostgreSQL</option>
    <option value="oracle" src="/images/dbs/oracle.png">Oracle</option>
    <option value="hive" src="/images/dbs/hive.png">Hive</option>
</select>
```

显示效果如下：

<select type="image" id="ImageSelector">
    <option value="mysql" src="@/images/dbs/mysql.png">MySQL</option>
    <option selected value="mariadb" src="@/images/dbs/mariadb.png">MariaDB</option>
    <option value="sqlserver" src="@/images/dbs/sqlserver.png">SQL Server</option>
    <option value="postgresql" src="@/images/dbs/postgresql.png">PostgreSQL</option>
    <option value="oracle" src="@/images/dbs/oracle.png">Oracle</option>
    <option value="hive" src="@/images/dbs/hive.png">Hive</option>
</select>

如上例，IMAGE 模式下 OPTION 标签一个专有属性`src`，用于设置图片的地址或路径。这时选项文字`text`将显示在图片的下方。

## 单选框模式

用来代替单选框 Radio 列表，仅需要将`type`属性设置为`radio`即可。

```html
<select type="radio" id="Radio1">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>
```

显示效果如下：

<select type="radio" id="Radio1">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>

## 复选框模式

用来代替单选框 Radio 列表，仅需要将`type`属性设置为`checkbox`即可。

```html
<select type="checkbox" id="Checkbox1">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>
```

显示效果如下：

<select type="checkbox" id="Checkbox1">
    <option value="mysql">MySQL</option>
    <option selected value="mariadb">MariaDB</option>
    <option value="sqlserver">SQL Server</option>
    <option value="postgresql">PostgreSQL</option>
    <option value="oracle">Oracle</option>
    <option value="hive">Hive</option>
</select>

## 加载数据

更多的情况下，我们需要从各种数据源加载数据，数据源也好，数据接口也好。SELECT 标签扩展通过`data`属性从其他数据源加载数据。

```html
<select data="1 to 10">
    <option>@item</option>
</select>

<select data='[{"lang": "Java", "id": "1"}, {"lang": "Scala", "id": "2"}, {"lang": "Python", "id": "3"}]'>
    <option value="@[id]">@[lang]</option>
</select>

<select data='{"Java": "1", "Scala": "2", "Python": "3"}'>
    <option value="@value">@key</option>
</select>

<select data="/api/languages">
    <option value="@[id]">@[lang]</option>
</select>
```

SELECT 数据加载使用 [FOR 标签](/root.js/model.md)提供支持，`data`属性的可选值与 FOR 标签的`in`属性相同，模板内容中的占位符规则也与 FOR 标签完全相同。

另外，使用`data`属性加载数据时没有默认选项，这时可以通过`value`属性来指定默认选项。

```html
<select data="1 to 10" value="2">
    <option>@item</option>
</select>
```

## 选择器

SELECT 组件是原生组件的扩展，可以使用选择器有：

* `$s(id)`，如`$s('#Select1')`，其中`#`不可省略。
* `$t(id)`，`id`中的`#`可省略，如`$t('Select1')`。
* `$select(id)`，同`$t(id)`。

上面 3 个选择器效果相同，都返回 Select 对象，详见说明见下一节。注意不能通过`$s('SELECT')`选择自定义标签。一般情况下用不到选择器，仅作为了解即可。

## 事件

通过事件可以让组件与用户进行交互，可用的事件有：

* `onchange` 当选项切换时触发。

```javascript
    $listen('name').on('change', function(before, current) {
        let text = before.text;
        let value = current.value;
        //......
    });
```

* `onchange+` 设置[服务器端事件](/root.js/server.md)。

```html
<select onchange+="/api/list?type={value}">
```

* `onload` 如果没有设置`data`属性，则当组件初始化完成之后触发；如果设置了`data`属性，则当数据加载完成之后触发。只触发一次。
* `onrelad` 当设置了`data`属性时有效，当数据重新加载完成之后触发，每次重新加载都触发。

## Select 类

除了在标签上设置的属性之外，还有几个可以在 Javascript 编程中使用的属性。

* `value` 返回或设置当前 SELECT 选中项的值。
* `text` 返回或设置当前 SELECT 选中项的文本。
* `selectedIndex` 返回或设置当前选中结果的索引，默认值为`-1`。在多选模式下返回第一个选中项的索引。
* `selectedIndexes` 在多选模式下，返回或设置当前选中结果的索引，默认值为`[]`。
* `options` 返回当前 SELECT 的所有选项的数组。数组元素类型为 `SelectOption`。

其他可以标签上设置的属性也可通过设置或调用。可用方法有：

* `reload()` 重新加载数据，当设置了`data`属性时才有意义。
* `set(attr, value)` 设置某个属性的值，可被[事件表达式](/root.js/event.md)使用，如`set-value-on="click: #Button1"`。

## SelectOption 类

除了上面介绍的可在标签上设置的属性之外，还有几个额外的属性可以通过 Javascript 调用。

* `index` 当前选项的索引，从`0`开始，只读。
* `selected` 当前选项是否被选中，例如`$select('Select1').options[0].selected = true;`。
* `disabled` 当前选项是否已禁用，例如`$s('#Select2').options[1].disbaled`。

可用的方法有：

* `remove()` 从 SELECT 选项列表中移除当前选项。


---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Model 数据加载模型](/root.js/model.md)


