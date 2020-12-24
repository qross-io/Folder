# Voyager 概览

Voyager与其他模板引擎如FreeMarker、Thymeleaf一样，实现动态数据在页面端的呈现，但 Voyager 更简单和直接。Voyager需要运行在Spring Boot环境下，适用于非前后端分离的项目（前后端分离的项目可使用[OneApi统一接口](/oneapi/overview.md)提供数据）。[**Master数据管理平台**项目](/master/overview.md)所有页面都使用Voyager开发。

Voyager基于嵌入式PQL实现，下面是一个简单的遍历数据的例子：
```html
<body>
<% FOR $name, $score IN (SELECT name, score FROM students) LOOP %>
    <div><%=$name%>: <%=$score%> - <%=IF $score > 60 THEN '# pass #' ELSE '# fail #' END %></div>
<% END LOOP %>
</body>
```

## 在页面上编写服务器端逻辑

像写 Jsp 或 php 一样将服务器端逻辑和页面逻辑写在一起，但是服务器端代码使用 **PQL**，使用`<%`和`%>`将服务器端代码包围起来（同Jsp、Asp等）。Voyager基于嵌入式PQL实现，支持所有的PQL语法规则。PQL是一种简单极致的数据处理语言，详见[PQL文档](/pql/overview.md)。

Voyager 负责在页面第一次加载时计算并呈现页面数据和内容，之后的与服务器的交互操作可通过 Javascript 和 OneApi 实现，更简单的接口开发方式请参见 [OneApi 文档](/oneapi/overview.md)。

## 不需要传递和绑定数据

因为可以在页面上直接访问数据库，所以不再需要在Controller中通过`model`向前端页面传递数据，减少了大量的后端逻辑。当然，Voyager仍然可以接收并通过变量的方式处理[`model`传递的数据](/voyager/model.md)。

## 极其简单的语法规则

上面示例中已经包含了Voyager的所有语法，即通过`<%`和`%>`包围服务器端语句，通过`#`和`#`包围多语言标签。详见[Voyager语法规则](/voyager/syntax.md)。

## 国际化支持

支持[多语言设置](/voyager/language.md)，语言设置保存在Cookie中，可由用户任意切换。


除了在页面开发可以使用外，Voyager 还支持邮件模板，可以在邮件发送前将动态页面计算完成之后再发送邮件。[PQL](/pql/send.md)、[DataHub邮件发送功能](/datahub/email.md)均支持 Voyager 动态模板，[Keeper调度工具](/keeper/overview.md)所有邮件模板均基于 Voyager 编写。


---
参考链接

* [使用Voyager](/voyager/use.md)
* [Voyager基本语法](/voyager/syntax.md)
* [通过Model向页面传递数据](/voyager/model.md)
* [多语言支持](/voyager/language.md)