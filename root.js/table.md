# TABLE 表格扩展

TABLE 对原生标签及 TABLE 内的子标签就行了扩展，以满足数据的展示、筛选和编辑需求。

TABLE 已是这个组件的第三个重构版本。第一版 DataTable 诞生于 2015年，为表格增加了表头过滤器等属性，可以和当时和 TdEditor 组件配合，用于数据的展示和复选。第二版 DataTable 诞生于 2018 年，为了 Master 项目重写，与 [Editor 组件](/root.js/editor.md)深度整合，一直用到现在。第三个版本在原生 TABLE 标签上进行了扩展，而不再是独立的类。

```html
<table id="Properties" width="100%" cellspacing="0" cellpadding="6" border="0" data="SELECT * FROM qross_properties">
    <colgroup>
        <col name="enabled">
        <col name="pid">
        <col name="title">
        <col name="property_source" map="local=磁盘上的配置文件&resources=项目资源目录中的配置文件&nacos=Nacos 配置中心&url=其他配置中心或服务">
        <col name="property_format" map="{ 'properties': 'Properties', 'yaml': 'Yaml', 'json': 'Json' }">
        <col name="property_path">            
    </colgroup>
    <thead>
        <tr>
            <th width="10%" class="left">启停用</th>
            <th width="5%" class="left">ID</th>
            <th width="20%" class="left">说明</th>
            <th width="15%" class="left">配置来源</th>
            <th width="10%" class="left">配置格式</th>
            <th width="40%" class="left">配置文件地址或 URL 路径</th>            
        </tr>
    </thead>
    <tbody>
    <template>
        <tr>
            <td><input type="switch" value="@[enabled]" onchange+="UPDATE qross_properties SET enabled='{value}', mender=@userid WHERE id=@[id]" /></td>
            <td sign="id">@[id]</td>
            <td><a href="/system/property?id=@[id]">@[title]</a></td>
            <td>@[property_source]</td>
            <td>@[property_format]</td>
            <td>@[property_path]?(&nbsp;)</td>
        </tr>
    </template>
    </tbody>
</table>
```

TABLE 组件必须设置`data`属性，`data`属性中可以设置接口或 PQL 语句以从后端获取数据。如果不需要从后端获取数据，只是为了使用 TABLE 的扩展属性，则`data`属性置空即可。`data`属性的更多信息请查看[与数据相关的属性](/root.js/data.md)。除了`data`之外，其他的扩展样式属性还有：

* `headerClass` 设置 THEAD 标签的 CSS 样式，默认值`header-row`。
* `footerClass` 设置 TFOOT 标签的 CSS 样式。
* `rowClass` 每一行的 CSS 样式，默认值`row`。
* `alternativeRowClass` 间隔行的 CSS 样式，默认值`alternate-row`
* `hoverRowClass` 鼠标划过行的 CSS 样式，默认值`row-hover`
* `activeRowClass` 鼠标点击行的 CSS 样式，默认值`row-active`
* `focusRowClass` 选中行的 CSS 样式，默认值`row-focus`
* `excludes` 指定哪一行不应用样式，使用`0`表示第一行，`1`表示第二行，依次类推。使用`n`或`l`表示最后一行，如`n-1`可表示倒数第二行。多个值之间使用逗号分隔。

与数据相关的其他属性还有：

* `auto-refresh` 是否启用数据自动刷新。
* `interval` 自动刷新间隔，单位毫秒，默认值为`2000`，即`2`秒。
* `terminal` 自动刷新停止的条件，默认为`false`，即永远不停止，这里输入 Javascript 表达式。与其他组件如 [MODEL](/root.js/model.md) 的`terminal`用法完全相同。
* `meet` 设置一个条件，在每次刷新时进行判断，当满足条件是触发`onmeet`事件，当不满足条件时触发`onmiss`事件。

与数据相关事件有：

* `onload` 第一次加载完成后触发。
* `onreload` 每次重新加载（刷新）完成后触发。
* `onmeet` 在每次刷新时，如果满足`meet`条件则触发，必须设置`meet`条件的才生效。
* `onmiss` 在每次刷新时，如果不满足`meet`设定的条件则触发，必须设置`meet`条件的才生效。

## 加载数据方式一

数据表格的意义是为了展示数据，TABLE 扩展后可以动态的从后端获取数据并显示到表格中。除了必须使用`data`属性外，还需要使用 TEMPLATE 设置数据模板，如上例所示。TEMPLATE 标签的语法规则见[数据加载模型](/root.js/model.md)。TEMPLATE 会将 data 属性返回的数据按行显示在表格中。如上例中 data 属性返回值的数据结构为:

```json
[
    {
        "id": 1,
        "title": "Hello World",
        "property_source": "nacos",
        "property_format": "properties",
        "property_path": "localhost:8090:group:prime"
    },
    {
        //....
    }
]
```

## 加载数据方式二

也可以不使用 TEMPLATE 标签加载数据，不过这种方式不如 TEMPLATE 灵活，但支持表格数据的自动无感刷新。这种方式不要设置 TEMPLATE 标签，而是在 COL 标签中设置`template`属性，如下列中所示。下例中所有两个`#`包含的内容为多语言符号，如`# actor-name #`，理解为文本即可。

```html
<table id="Actors" width="92%" cellpadding="6" cellspacing="0" auto-refresh="yes" meet="this.querySelector('td.green') == null" onmeet-="fadeOut: #LoadingHint, #RestartButtonDiv" onmiss-="fadeIn: #RestartButtonDiv" interval="2000" data="/api/keeper/beats?node_address=<%=$node.node_address%>">
    <colgroup>
        <col name="Actor" class="bold l180" template="@[actor_name]" map='{ "Keeper": "# actor-keeper-title #", "Inspector": "# actor-inspector-title #", "TaskProducer": "# actor-task-producer-title #", "TaskStarter": "# actor-task-starter-title #", "TaskChecker": "# actor-task-checker-title #", "TaskExecutor": "# actor-task-executor-title #", "TaskLogger": "# actor-task-logger-title #", "NoteProcessor": "# actor-note-processor-title #", "NoteQuerier": "# actor-note-querier-title #", "Repeater": "# actor-repeater-title #" }'>
        <col name="Status" class="~{ '@[status]' == 'running' ? 'green' : 'red' }" template="@[status]" map="{ 'rest': '# status-rest #', 'running': '# status-running #' }">
        <col name="StartTime" template="@[start_time]">
        <col name="QuitTime" template="@[quit_time]?(N/A)">
        <col name="LastBeatTime" template="@[last_beat_time]">
    </colgroup>
    <thead>
        <tr>
            <th class="left f16 l180" width="30%"># actor-name #</th>
            <th class="left f16" width="16%"># actor-status #</th>
            <th class="left f16" width="18%"># start-time #</th>
            <th class="left f16" width="18%"># quit-time #</th>
            <th class="left f16" width="18%"># last-beat-time #</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>
```

几点说明：

* COL 标签的`template`属性占位符规则与 TEMPLATE 标签相同。
* COL 标签的`class`属性用来设置内容单元格的样式，而不是 COL 的样式，支持 [Express 表达式](/root.js/express.md)。
* TBODY 标签需要设置，如果有多个 TBODY 标签，那么则在第一个 TBODY 标签中显示数据。

一起说一下`meet`属性，这里的`meet`属性设置了一个 Javascript 表达式，`this`指向当前表格。当找不到`td.green`时，触发`onmeet`事件，否则触发`onmiss`事件。


## 使用字典数据

上面第一个示例例中，`property_source`和`property_format`两个字段都使用字典数据，在数据库保存的是`nacos`和`properties`，但是这两个数据库保存的值并不容易理解，甚至数据库中经常存储数字格式的字典值。这种情况下就需要将字典值翻译成容易理解的文字，使用 COL 标签的`map`属性可以完成这个功能。如上面第一例所示，`map`属性可以设置字典型对应的文本，当表格加载时，自动将字典值翻译成对应的文本。`map`属性支持 Json 对象和查询字符串两种格式的配置。配置之后`nacos`和`properties`对应的文本分别是`Nacos配置中心`和`Properties`。第二个示例也在 COL 标签中使用了`map`属性用于对值进行转换。注意转换是单元格中整个内容的转换。


TABLE 还有更多功能说明会不断补全。

---
参考链接

* [与数据相关的属性](/root.js/data.md)
* [数据加载模型](/root.js/model.md)