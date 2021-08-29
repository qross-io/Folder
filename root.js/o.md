# O 标签

O 标签属于 [Model 数据加载模型](/root.js/model.md)的一部分，引用文件和 Model 引用的文件相同。MODEL 标签可以一次查询然后将数据显示在不同的标签（元素）的属性中，经常我们查询只使用一次，所以框架提供了 **O 标签**用于实现一次性数据查询需求。O 标签在 **加载完成后自动移除**，只保留加载的文本数据。`O`是`output`的首字母。

第一种应用方式，从接口或 PQL 中加载数据。

```html
<o>SELECT title FROM table WHERE id=#{id} -> FIRST CELL</o>
```

作者在这种情况下一般使用 [Voayger 模板引擎](/voyager/overview.md) 的服务器端代码实现。

```sql
<%=SELECT title FROM table WHERE id=#{id} -> FIRST CELL%>
```

不同点是前者是在客户端实现的异步查询，在页面呈现给用户之后执行；后者是在服务器端实现的查询，是在页面呈现给用户之前。O 标签与[data 属性](/root.js/data.md)实现的逻辑完全相同，也支持接口或跨域接口。

```html
<o>/api/page/title?id=#{id} -> /data</o>
<o>SELECT title FROM table1 WHERE id=$id -> FIRST CELL</o>
```

第二种应用方式，从 Model 中加载数据，这种方式必须以`@`符号开头。

```html
<model name="page" data="/api/page/title?id=#{id} -> /data"></model>

文章标题：<o>@page.title</o>; 文章浏览：<o>@page.views?(0)</o>
```

第三种应用方式，从`data`属性加载数据。O 标签的数据占位符规则为 `@:keyOrPath?(defaultValue)`。

```html
<o data="/api/page/title?id=#{id} -> /data">文章标题：@:title; 文章浏览：@:views?(0)</o>
```

其中的`@:`表示 O 标签从后端加载的数据，请参阅[数据占位符](/root.js/holder.md)获取完整规则信息。

因为除了 [SPAN 标签](/root.js/span.md)之外，其他标签不支持从后端获取数据。O 标签让任意元素内的数据输出成为可能。可以把 O 标签嵌套在任意其他标签中，实现数据加载需求。而且 O 标签在加载完数据后，会自动移除，不会对页面布局和其他元素的样式产生任何影响。

```html
<h1><o>@page.title</o></h1>
```

O 标签的缺点一是不支持自动刷新，需要的话可以使用 [MODEL 标签](/root.js/model.md)或 [TEMPLATE 标签](/root.js/template.md)实现；二是不支持重新加载，可以使用 [SPAN 标签](/root.js/span.md)实现。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [与数据相关的属性](/root.js/data.md)
* [SPAN 标签扩展](/root.js/span.md)
* [TEMPLATE 标签](/root.js/template.md)
* [数据占位符](/root.js/holder.md)