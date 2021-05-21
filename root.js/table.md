# TABLE 表格扩展

TABLE 对原生标签及 TABLE 内的子标签就行了扩展，以满足数据的展示、筛选和编辑需求。

TABLE 已是这个组件的第三个重构版本。第一版 DataTable 诞生于 2015年，为表格增加了表头过滤器等属性，可以和当时和 TdEditor 组件配合，用于数据的展示和复选。第二版 DataTable 诞生于 2018 年，为了 Master 项目重写，与 [Editor 组件](/root.js/editor.md)深度整合，一直用到现在。

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

TABLE 组件必须设置`data`属性，`data`属性中可以设置接口或 PQL 语句以从后端获取数据。如果不需要从后端获取数据，只是为了使用 TABLE 的扩展属性，则`data`属性置空即可。`data`属性的更多信息请查看[与数据相关的属性](/root.js/data.md)。除了`data`之外，其他的扩展属性还有：

* `headerClass` 设置 THEAD 标签的 CSS 样式，默认值`header-row`。
* `footerClass` 设置 TFOOT 标签的 CSS 样式。
* `rowClass` 每一行的 CSS 样式，默认值`row`。
* `alternativeRowClass` 间隔行的 CSS 样式，默认值`alternate-row`
* `hoverRowClass` 鼠标划过行的 CSS 样式，默认值`row-hover`
* `activeRowClass` 鼠标点击行的 CSS 样式，默认值`row-active`
* `focusRowClass` 选中行的 CSS 样式，默认值`row-focus`

## 加载数据

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

## 使用字典数据

上例中，`property_source`和`property_format`两个字段都使用字典数据，在数据库保存的是`nacos`和`properties`，但是这两个数据库保存的值并不容易理解，甚至数据库中经常存储数字格式的字典值。这种情况下就需要将字典值翻译成容易理解的文字，使用 COL 标签的`map`属性可以完成这个功能。如上例所示，`map`属性可以设置字典型对应的文本，当表格加载时，自动将字典值翻译成对应的文本。`map`属性支持 Json 对象和查询字符串两种格式的配置。配置之后`nacos`和`properties`对应的文本分别是`Nacos配置中心`和`Properties`。


TABLE 的更多功能说明会不断补全。

---
参考链接

* [与数据相关的属性](/root.js/data.md)
* [数据加载模型](/root.js/model.md)