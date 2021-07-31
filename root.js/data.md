# 与数据相关的属性

在前端开发中，我们经常需要从后端获取数据，root.js 为组件或扩展标签提供了一些数据属性用于向服务器端发送请求或设置静态数据。数据属性是与数据相关的组件的应用基础，可以说是数据相关组件最关键的属性。比如`data`属性是一般用于异步地从数据源加载数据，发送查询请求，还有一些其他的属性用于发送非查询请求，用于与用户交互。

```html
<table data="select * from students">...</table>
<select data="/api/options"></select>
<span editable="yes" onchange+="put:/api/student/name?id=&(id)&name="></span>
<input type="text" onblur+="select id from users where username='{value}'" />
<button id="Btn1" onclick+="insert into table1 (name, age) values ('$(#name)', $(#age))">
<textarea coder="java" onsave+="/api/code/update?id=&(id)&code="></textarea>
```

数据属性支持多种数据类型，比较典型的是接口和 PQL，也可以是静态数据。

## 数据接口

这里仅支持 Json 数据格式的接口，数据接口的书写格式为 `method:url # path -> expection`。其中

* `method`是请求方法，如果是`GET`时则可以省略，不区分大小写
* `url`是 URL 地址，可使用相对或绝对地址（需要后端预先实现中转接口）
* `path`是返回数据后的解析路径，默认`/`，这里支持标准 JsonPath 格式，必须以`/`开头，可以省略
* `expection`用来判断接口返回值是否符合预期，仅服务器端事件支持这个设置，详见[服务器端事件](/root.js/server.md)接口部分的说明。

示例
```html
<template data="post:https://www.domain.com/api/students -> /data -> size > 0">...</template>
<for in="https://www.domain.com/api/scores?grade=7">...</for>
```

FOR 标签的`in`属性也是和`data`属性一样的逻辑。接口地址支持 [Express 字符串](/root.js/express.md)语法，可嵌入地址参数或 DOM 操作符。另外，为了方便调用接口，root.js 可以对[请求接口的逻辑进行全局设置](/root.js/ajax.md)，以简化接口的调用过程。

## PQL 语句

可以在数据属性中直接写 PQL 语句，因为查询逻辑一般比较简单，所以通常表现为一条 SELECT 语句。这里支持所有的 PQL 语句规则，详见 [PQL文档](/pql/overview)。

```html
<model name="studens" data="select * from students"></model>
<table data="open mysql.school; select * from students">
<span data="select title from articles where id=7725 -> first cell">文章标题：@:[title]</span>
```

这里的 PQL 语句 也由 [Express 字符串](/root.js/express.md)解析，可嵌入地址参数或 DOM 操作符。

## 静态数据

数据属性不仅可以通过接口或 PQL 语句异步的从数据源加载数据，也可以直接在属性里直接写 Json 格式的数据。

```html
<for in='[{"name": "Tom", "age": 8}, {"name": "Jerry", "age": 9}]'>...</for>
```

这个场景虽然用得少，但有些情况下还有非常好用的。

## 传递数据

大多数情况下都需要向数据属性中传递参数值，数据属性支持 [Express 字符串](/root.js/express.md)支持的所有参数格式。

```html
<select onchange+="/api/select/change?item=$:[text]" />
<input onchange+="select id from users where username='{value}'">
```

---
参考链接

* [服务器端事件]](/root.js/server.md)
* [Express 字符串](/root.js/express.md)
* [Model 数据加载模型](/root.js/model.md)
* [Ajax 接口请求全局设置](/root.js/ajax.md)