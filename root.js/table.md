# TABLE 表格扩展

对原生 TABLE 标签及 TABLE 内的子标签就行了扩展，以满足数据的展示、筛选和编辑需求。TABLE 标签扩展的主要功能有：

* 支持通过后端接口、Model 数据模型或PQL 语句获取数据，也支持各种静态数据。
* 支持数据自动刷新或重新加载，支持无感刷新。
* 支持使用数据字典格式化列数据。
* 支持按列进行单元格数据编辑。
* 支持表头筛选数据。
* 支持表头排序。
* 支持行选中、鼠标划入行、鼠标划出行等相关的事件。
* 支持双击行触发的服务器端事件。
* 支持与数据加载相关的事件。
* 支持间隔行、选中行相关的样式设置。

TABLE 已是这个组件的第三个重构版本。第一版 DataTable 诞生于 2015年，为表格增加了表头过滤器等属性，可以和当时和 TdEditor 组件配合，用于数据的展示和复选。第二版 DataTable 诞生于 2018 年，为了 Master 项目重写，与 [Editor 组件](/root.js/editor.md)深度整合，一直用到现在。第三个版本在原生 TABLE 标签上进行了扩展，而不再是独立的类。

## 使用

TABLE 扩展需要以下相关文件：

* `root.js` [基础库](/root.js/root.md)
* `root.model.js` [数据加载模型](/root.js/model.md)
* `root.table.js` 主文件
* `css/root/table.css` 默认样式
* `root.animation.js` [动画库](/root.js/animation.md)，交互过程中提示文字需要用到。

## TABLE 属性

TABLE 组件必须设置`data`属性，`data`属性中可以设置接口或 PQL 语句以从后端获取数据。如果不需要从后端获取数据，只是为了使用 TABLE 的扩展属性，则`data`属性置空即可。`data`属性的更多信息请查看[与数据相关的属性](/root.js/data.md)。除了`data`之外，与数据相关的其他扩展属性还有：

* `auto-refresh` 是否启用数据自动刷新。
* `interval` 自动刷新间隔，单位毫秒，默认值为`2000`，即`2`秒。
* `terminal` 自动刷新停止的条件，默认为`false`，即永远不停止，这里输入 Javascript 表达式。与其他组件如 [MODEL](/root.js/model.md) 的`terminal`用法完全相同。
* `meet` 设置一个条件，在每次刷新时进行判断，当满足条件是触发`onmeet`事件，当不满足条件时触发`onmiss`事件。

其他的扩展样式扩展属性有：

* `header-class` 设置 THEAD 标签的 CSS 样式，默认值`header-row`。
* `footer-class` 设置 TFOOT 标签的 CSS 样式。
* `row-class` 每一行的 CSS 样式，默认值`row`。
* `alternative-row-class` 间隔行的 CSS 样式，默认值`alternate-row`
* `hover-row-class` 鼠标划过行的 CSS 样式，默认值`row-hover`
* `active-row-class` 鼠标点击行的 CSS 样式，默认值`row-active`
* `focus-row-class` 选中行的 CSS 样式，默认值`row-focus`
* `excludes` 指定哪一行不应用样式，使用`0`表示第一行，`1`表示第二行，依次类推。使用`-1`表示最后一行，`-2`可表示倒数第二行，依次类推。多个值之间使用逗号分隔。

所有默认样式在`/css/root/table.css`中找到。

与[服务器端事件](/root.js/server.md)提示信息相关的扩展属性有：

* `success-text` 当`onrowdblclick+`事件执行符合预期时提醒的文字。
* `failure-text` 当`onrowdblclick+`事件执行不符合预期时提醒的文字。
* `exception-text` 当`onrowdblclick+`事件执行出错提醒的文字，默认会提醒出错信息。

出错信息会使用 Message 的方式进行提醒，如果没有引用`root.animation.js`，则上面的设置不会生效。

与排序和筛选相关的属性有：

* `reload-on-filter-or-sorting` 是否当筛选或排序时重新加载数据，当数据从服务器端获取时一般需要启用这个选项。
* `reload-on-filter` 是否仅当筛选时重新加载数据。
* `reload-on-sorting` 是否仅当排序时重新加载数据。

TABLE 标签的原生属性不在这里赘述。

## COL 属性

因为基于表格列要增加额外的功能，如筛选、排序或数据编辑等，所以作者对 COL 标签也进行了扩展。新增的扩展属性有：

* `name` 列名，强烈建议设置。
* `editable` 是否启用编辑，使用可识别的布尔值，如`editable="yes"`。
* `filterable` 是否启用筛选。
* `sortable` 是否启用排序。
* `template` 单元格数据模板，见下文“数据加载方式二”中的说明。
* `map` 数据字典，见下文中的说明。 

## 事件

与其他标签一样，TABLE 标签的事件可以在元素标签上定义，也可以使用事件绑定。作者建议在元素标签上定义，以尽可能减少 Javascript 的编写。

与数据相关事件有：

* `onload` 第一次加载完成后触发。
* `onreload` 每次重新加载（刷新）完成后触发。
* `onmeet` 在每次刷新时，如果满足`meet`条件则触发，必须设置`meet`条件的才生效。
* `onmiss` 在每次刷新时，如果不满足`meet`设定的条件则触发，必须设置`meet`条件的才生效。

与数据行相关的事件如下，事件函数均为`function(row) {}`，其中参数`row`为相关行：

* `onrowhover` 鼠标划过行时触发。
* `onrowleave` 鼠标划出行时触发。
* `onrowfocs` 行被选中时触发。
* `onrowblur` 行被取消选中时触发。
* `onrowdblclick` 双击时触发。
* `onrowdelete` 行被删除时触发，支持`return false`取消删除。

其中`onrowdbclick`支持[服务器端交互](/root.js/server.md)。

* `onrowdblclick+` 服务器端事件。
* `onrowdblclick+success` 服务器端逻辑执行符合预期时触发。
* `onrowdblclick+failure` 服务器端逻辑执行不符合预期时触发。
* `onrowdblclick+exception` 服务器端逻辑执行出错时触发。
* `onrowdblclick+completion` 服务器端逻辑执行完成时触发，无论结果如何。

与排序和筛选相关的事件有：

* `onfilter` 当执行筛选时触发。
* `onfiltercreset` 当重置筛选时触发。
* `onsort` 当执行排序时触发。

另外，所有非服务器端事件都支持[精简事件表达式](/root.js/event.md)，下文的例子中会涉及一些。

## 加载数据方式一

数据表格的意义是为了展示数据，TABLE 扩展后可以动态的从后端获取数据并显示到表格中。除了必须使用`data`属性外，还需要使用 TEMPLATE 设置数据模板，如上例所示。TEMPLATE 标签的语法规则见[数据加载模型](/root.js/model.md)。TEMPLATE 会将`data`属性返回的数据按行显示在表格中。假如`data`属性返回值的数据结构为:

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

代码示例：

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

## 加载数据方式二

也可以不使用 TEMPLATE 标签加载数据，不过这种方式不如 TEMPLATE 灵活，但支持表格数据的自动无感刷新。这种方式不要设置 TEMPLATE 标签，而是在 COL 标签中设置`template`属性，如下列中所示。下例中所有两个`#`包含的内容为多语言符号，如`# actor-name #`，理解为文本即可。

```html
<table id="Actors" width="92%" cellpadding="6" cellspacing="0" auto-refresh="yes" meet="this.querySelector('td.green') == null" onmeet-="fadeOut: #LoadingHint, #RestartButtonDiv" onmiss-="fadeIn: #RestartButtonDiv" interval="2000" data="/api/keeper/beats?node_address=<%=$node.node_address%>">
    <colgroup>
        <col name="Actor" class="bold l180" data-format="@[actor_name]" map='{ "Keeper": "# actor-keeper-title #", "Inspector": "# actor-inspector-title #", "TaskProducer": "# actor-task-producer-title #", "TaskStarter": "# actor-task-starter-title #", "TaskChecker": "# actor-task-checker-title #", "TaskExecutor": "# actor-task-executor-title #", "TaskLogger": "# actor-task-logger-title #", "NoteProcessor": "# actor-note-processor-title #", "NoteQuerier": "# actor-note-querier-title #", "Repeater": "# actor-repeater-title #" }'>
        <col name="Status" class="~{ '@[status]' == 'running' ? 'green' : 'red' }" data-format="@[status]" map="{ 'rest': '# status-rest #', 'running': '# status-running #' }">
        <col name="StartTime" data-format="@[start_time]">
        <col name="QuitTime" data-format="@[quit_time]?(N/A)">
        <col name="LastBeatTime" data-format="@[last_beat_time]">
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

下面是上例的几点说明：

* COL 标签的`template`属性占位符规则与 TEMPLATE 标签相同。
* COL 标签的`class`属性用来设置内容单元格的样式，而不是 COL 的样式，支持 [Express 表达式](/root.js/express.md)。
* TBODY 标签需要设置，如果有多个 TBODY 标签，那么则在第一个 TBODY 标签中显示数据。

一起说一下`meet`属性，这里的`meet`属性设置了一个 Javascript 表达式，`this`指向当前表格。当找不到`td.green`时，触发`onmeet`事件，否则触发`onmiss`事件。


## 使用字典数据

上面第一个示例中，`property_source`和`property_format`两个字段都使用字典数据，在数据库保存的是`nacos`和`properties`，但是这两个数据库保存的值并不容易理解，甚至数据库中经常存储数字格式的字典值。这种情况下就需要将字典值翻译成容易理解的文字，使用 COL 标签的`map`属性可以完成这个功能。如上面第一例所示，`map`属性可以设置字典型对应的文本，当表格加载时，自动将字典值翻译成对应的文本。`map`属性支持 Json 对象和查询字符串两种格式的配置。配置之后`nacos`和`properties`对应的文本分别是`Nacos配置中心`和`Properties`。第二个示例也在 COL 标签中使用了`map`属性用于对值进行转换。注意转换是单元格中整个内容的转换。

## 筛选

如果在列上启用了筛选，即`<col filterable="yes">`，并且还设置了表头（对应的 TH 标签），则表头上会显示一个过滤器图表，双击表头可启动筛选输入。目前只支持输入关键词的筛选，更多功能会在未来加入。来看一下示例：

```html
<table id="Users" datatable="yes" width="100%" cellpadding="8" cellspacing="0" border="0"
            fullname="" email="" reload-on-filter-or-sorting="yes"
            data="/api/user/filter?fullname=$:[fullname]?()&email=$:[email]?()"
            onrowdblclick+="post:/api/user/member?team_id=&(id)&leader_id=<%=$team_leader%>&user_id=$:[user] -> not-zero"
            onrowdblclick+success-="delete-row; reload: #Members"
            failure-text="Duplicate member in team.">
        <colgroup>
            <col name="fullname" filterable="yes">
            <col name="email" filterable="yes">
            <col name="character" map="worker=# global.worker #&leader=# global.leader #&keeper=# global.keeper #&master=# global.master #">
        </colgroup>
        <thead>
        <tr>
            <th width="25%" class="left">FullName</th>
            <th width="50%" class="left">Email</th>
            <th width="25%" class="left">Role</th>
        </tr>
        </thead>
        <tbody>
            <template>
                <tr user="@[id]">
                    <td>@[fullname]</td>
                    <td>@[email]</td>
                    <td>@[role]</td>
                </tr>
            </template>
        </tbody>
    </table>
</div>
```

关于筛选的实现逻辑解释如下：

* 先看 COL 标签，列`fullname`和`email`通过设置`filterable`属性启用了筛选，`<col name="fullname" filterable="yes">`。
* 通过 TH 设置了表头`FullName`和`Email`，会在表头文字后面显示一个漏斗图标。
* 当双击表头`FullName`或`Email`，会在对应列`fullname`和`email`显示文本输入框。
* 在文本框内输入文字后敲击回车提交筛选，如果`reload-on-filter-or-sorting`为`false`，则在客户端过滤数据。
* 如果`reload-on-filter-or-sorting`为`true`，则清空所有数据并向后端提交请求，重新加载`data`中的数据。
* 提交筛选后，会自动在 TABLE 标签上设置对应列名的属性`fullname`和`email`，并将输入值保存在这个属性中，上例中是`fullname=""`。
* `data`属性可以获取这两个属性的值，例中是`@:[fullname]`和`@:[email]`，`@:`表示当前标签即 TABLE，更多语法请参阅 [Express 字符串](/root.js/express.md)。
* 再次从`data`获取的数据会重新呈现在表格中。

所以说，表格的列名非常重要，不只为列添加区分标识，也在筛选过程中起传递数据的作用。

## 排序

文档待完善。

## 服务器端事件

以筛选的例子解释一下服务器端事件`onrowdblclick+`的交互过程，例子中设置了一个服务器端事件`onrowdblclick+`和一个精简事件`onrowdblclick+success-`，还设置了事件相关属性`failure-text`，表示一个当请求结果不符合预期时的提示文字。事件执行逻辑如下：

* 当用户双击表格的某行时，如果客户端事件`onrowdblclick`返回结果为`true`或不返回任何值时触发对应的服务器端事件`onrowdblclick+`。
* 服务器端事件请求其设置的接口`post:/api/user/member?team_id=&(id)&leader_id=<%=$team_leader%>&user_id=$:[user] -> not-zero`。
* 接口使用 POST 请求；`&(id)`表示从页面地址中取`id`参数的值；`<%=$team_leader%>`是 [Voyager 模板引擎](/voyager/overview.md)的服务器端语法，可忽略；`$:[user]`表示取当前元素的`user`属性的值，与表格行相关的事件当前元素都指定当前行而不是整个 TABLE。
* 接口中的`-> not-zero`，表示当结果返回值不为`0`时表示结果正确或符合预期。因为服务器端事件一般都为非查询操作，一般都返回受影响的行数。当返回结果为`0`时表示执行结果不正确或不符合预期，而大于`0`时则表示执行符合预期。
* 由于设置了`faliure-text`属性，所以当执行结果不正确时，则提示`faliure-text`属性中的文字`Duplicate member in team.`。提示文字会以 [Message 的形式](/root.js/animation.md)显示。
* 当执行结果正确时，执行客户端事件`onrowdblclick+success`，例中使用了精简事件`onrowdblclick+success-`。其中`delete-row`表示删除当前行，也可以是`delete`；`reload: #Members` 表示重新加载另一个标签`#Members`。

通过服务器端事件及相关属性，可以不用编写 Javascript 的而实现相应的后端处理逻辑。


## 单元格编辑

文档待完善。



---
参考链接

* [与数据相关的属性](/root.js/data.md)
* [数据加载模型](/root.js/model.md)
* [数据占位符](/root.js/data-placeholder.md)
* [Express 字符串](/root.js/express.md)
* [Message 消息组件](/root.js/animation.md)