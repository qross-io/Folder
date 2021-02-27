# 标签自定义属性 data

**data** 属性是 root.js 组件库对标签属性的扩展，一般用于自定义或扩展标签中，用于异步地从数据源加载数据。data 属性是与数据相关的组件的应用基础，可以说是数据组件最关键的属性。还有另外两个属性具有与 data 属性相同的功能或处理逻辑，一个是 FOR 标签的 **in** 属性，另一个是特定标签的交互属性 **action**，在按钮、可编辑标签中会出现。

```html
<table data="select * from students">...</table>
<select data="/api/options"></select>
<span editable="yes" action="put:/api/student/name?id=&(id)&name="></span>
<button id="Btn1" action="insert into table1 (name, age) values ('$(@name)', $(#age))">
```

data 属性支持多种数据类型，比较典型的是接口和 PQL，也可以是静态数据。

## 数据接口

这里仅支持 Json 数据格式的接口，数据接口的书写格式为 `method:url -> path`。其中

* `method`是请求方法，如果是`GET`则可以省略，不区分大小写
* `url`是 URL 地址，可使用相对或绝对地址（需要后端预先实现中转接口）
* `path`是返回数据后的解析路径，默认`/`，这里支持标准 JsonPath 格式，可以省略

示例
```html
<template data="post:https://www.domain.com/api/students -> /data">...</template>
<for in="https://www.domain.com/api/scores?grade=7">...</for>
```

FOR 标签的`in`属性也是和 data 属性一样的逻辑。

地址字符串内容支持 [Express 字符串](/root.js/express.md)语法，可嵌入地址参数或 DOM 操作符。

## PQL 语句

可以在 data 属性中直接写 PQL 语句，因为查询逻辑一般比较简单，所以通常表现为一条 SELECT 语句。这里支持所有的 PQL 语句规则，详见 [PQL文档](/pql/overview)。

```html
<model name="studens" data="select * from students">
<table datatable="yes" data="open mysql.school; select * from students">
<span data="select title from articles where id=7725 -> first cell">文章标题：@:[title]</span>
```

这里的 PQL 语句 也由 [Express 字符串](/root.js/express.md)解析，可嵌入地址参数或 DOM 操作符。

## 静态数据

data 属性不仅可以通过接口或 PQL 语句异步的从数据源加载数据，也可以直接在 data 属性里直接写 Json 格式的数据。

```html
<for in='[{"name": "Tom", "age": 8}, {"name": "Jerry", "age": 9}]'>...</for>
```

这个场景虽然用得少，但有些情况下还有非常好用的。


---
参考链接

* [Express 字符串](/root.js/express.md)
* [Model 数据加载模型](/root.js/model.md)