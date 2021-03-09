# Select 组件扩展

**Select** 组件扩展了原生 SELECT 标签，增加了多种选择模式，并提供多选功能。

## 使用

Select 组件的依赖库和组件列表如下：

* [root.js 基础库](/root.js/root.md)
* [Model 数据加载模型](/root.js/model.md)
* Select 组件默认样式表 `root.select.css`

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
     multiple="boolean" data="api" action="api">
    <option value="" title="" class="CSS" selected-class="CSS" disabled-class="CSS">text</option>
    ...
</select>
```

除支持所有原生属性外，SELECT 标签增加的扩展属性如下：

* `type` 选择框的模式，现在支持`button`和`image`两种模式，以后会增加更多模式，下面会分别介绍。如果没有这个属性则保持原始模式。
* `class` 整个框体的 CSS 样式，不同模式下框体的标签元素不同。
* `option-class` 每个选项的 CSS 样式，根据不同的 type 默认值不同。
* `selected-option-class` 被选中选项的 CSS 样式，根据不同的 type 默认值不同。
* `disabled-option-class` 禁用状态选项的 CSS 样式，根据不同的 type 默认值不同。
* `multiple` 原生属性，是否支持多选，支持设置可识别为布尔值的所有字符串，如`yes`、`FALSE`、`1`等，默认`false`。
* `data` 从其他数据源生成 OPTION 列表，暂时不可用。
* `action` 当切换选项时执行的操作，支持接口和 PQL 语句。

除支持所有原生属性外，OPTION 标签增加的扩展属性如下：

* `title` 在鼠标划过选项时的提示文字。
* `class` 选项的默认样式，不同模式下框体选项的标签元素不同。
* `selected-class` 选项在被选中时的样式。
* `disabled-class` 选项在禁用状态时的样式。
* `show` 当前选项被选中时，需要显示的元素，这里输入选择器，如`show="#A,#B"`。
* `hide` 当前选项被选中时，需要隐藏的元素，这里的值也是选择器，如`hide='span.hint'`。一般与`show`属性同时设置，`hide`先与`show`执行。

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

## 加载数据

暂无。

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

其他事件暂缺。

## Select 类

除了在标签上设置的属性之外，还有几个可以在 Javascript 编程中使用的属性。

* `value` 返回当前 SELECT 选中项的值，只读。
* `text` 返回当前 SELECT 选中项的文本，只读。
* `selectedIndex` 返回或设置当前选中结果的索引，默认值为`-1`。
* `options` 返回当前 SELECT 的所有选项的数组。数组元素类型为 `SelectOption`。

其他可以标签上设置的属性也可通过设置或调用。可用方法暂缺。

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


