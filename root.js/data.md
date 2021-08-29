# 与数据相关的属性

在前端开发中，我们经常需要从后端获取数据或与后端进行交互，root.js 为组件或扩展标签提供了一些数据属性用于向服务器端发送请求或设置静态数据。数据属性是与数据相关的组件的应用基础，可以说是数据相关组件最关键的属性。比如`data`属性是一般用于异步地从数据源加载数据，发送查询请求，[服务器端事件](/root.js/server.md)用于发送非查询请求，用于与用户交互。

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
<span data="select title from articles where id=7725 -> first row">文章标题：@:title</span>
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

## 加载等待

实际应用中，一个页面经常会有多个需要加载数据的组件，这个组件有时是有依赖关系的，比如 B 组件依赖于 A 组件。Ajax 本身的后端请求机制是异步的，即使是 B 组件在页面上的位置在 A 组件之后，A、B 两个组件会依次开始加载，而不是 B 组件等待 A 组件加载完成后再加载。这时，如果 B 组件的加载请求依赖于 A 组件返回的数据，就会出现问题，比如 SELECT 标签的两级联动，下级 SELECT 依赖于上级 SELECT 的数据。为了解决这个问题，root.js 标签库为需要加载数据的标签都提供了`await`属性，即有`data`属性的标签都支持。来看例子：

```html
<select id="Select1" data="...">...</select>
<select id="Select2" await="#Select1" data="...?key=$(#Select1)">...</select>
<select id="Select3" await="#Select1,#Select2" data="...?key=$(#Select1)&value=$(#Select2)">...</select>
```

如上，Select2 加载需要使用 Select1 的数据，Select3 需要使用 Select1 和  Select2 的数据。Select2 的`await`属性表示等待 Select1 加载完成后再加载，Select3 的`await`属性表示等待 Select1 和  Select2 加载完成后再加载。如果不使用`await`属性，则三个组件会依次加载，当 Select2 加载时 Select1 并没有加载完成，Select3 加载时 Select1 和 Select2 也没有加载完成，所以会直接报错。

---
参考链接

* [服务器端事件](/root.js/server.md)
* [Express 字符串](/root.js/express.md)
* [Model 数据加载模型](/root.js/model.md)
* [Ajax 接口请求全局设置](/root.js/ajax.md)