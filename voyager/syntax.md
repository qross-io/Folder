# Voyager 语法

Voyager 的嵌入语法非常简单，一共有`4`种。

## 用`<%`和`%>`包围的 PQL 代码

PQL变量、语句、程序块均可使用`<%`和`%>`包围，这个语法和 Jsp 或 Asp 是一样的。例如：

```html
<body>
<%
OPEN mysql.classes;

SET $id := 1;
SET $name := 'Tom';
%>
<h1><% OUTPUT $name; %></h1>
<% IF $id == 1 THEN %>
    <div>
        <%
        VAR $course := SELECT * FROM students WHERE id=$id;
        OUTPUT $course.name;
        %>: 
        <span class="score"><%=$course.score%></span>
    </div>
<% ELSE %>
    <div>
        <%=SELECT * FROM students -> TO HTML TABLE %>
    </div>
<% END IF%>
<hr>
<% FOR $i IN 1 TO 12 LOOP %>
    <span><%=$i%></span>
<% END IF %>
<script type="text/javascript">
<% IF @cookies.role == 'teacher' THEN%>
    $s('.score').onclick = function() { ... };
<% END IF %>
</script>
</body>
```

以下各部分的解释如下：

* `<%`和`%>`可以包围一行代码，也可以包含多行代码。多行代码之间使用分号`;`隔开，包含一行代码或多行代码的最后一行代码可省略行尾的分号`;`。如上面的`<% OUTPUT $name; %>`中以省略分号。
* `<%=`和`%>`用于输出单个值，是 [OUTPUT 语句](/pql/output.md)的简写形式，如上面的`<% OUTPUT $name; %>`可以简写为`<%=$name%>`，注意末尾不加分号。
* `<%=` 后面可以跟表达式，最后会输出表达式的计算结果。上例`<%=SELECT * FROM students -> TO HTML TABLE %>`会将查询结果生成一个 HTML 表格。
* 条件语句或循环语句的条件体或循环体语句中可以使用 HTML 语句块，类似上面的 [IF 语句](/pql/if.md)和 [FOR 语句](/pql/for.md)
* `<%`和`%>`可以嵌入到 HTML 中，也可以嵌入到 Javascript 代码中，即页面的任何部分都可以嵌入。

## 服务器端包含 Server Side Include

Voyager 支持在页面中包含其他页面碎片或 SQL 文件，包含进来后作为整个页面的一部分。例如：

```html
<#include file="/bar.html"/>
```

引入文件的路径相对于当前站点，即`resources/templates`目录，引于的页面碎片文件不需要添加对应的`Controller`。

也可以引入`.sql`扩展名的 PQL 文件，比如公用的逻辑可以单独放在一个文件中。例如：

```html
<#include file="/job/rules.sql"/>
```

另外一种语言配置文件的引入在[多语言支持](/voyager/language.md)中介绍。服务器端包含支持传递参数，格式同 URL 地址传参，目的是对包含页面进行配置，详见 [Voayger 使用参数](/voyager/query.md)。

## 使用母版页 Master Page

```html
<#page template="/master/form.html?name=Tom&age=18"/>
```

母版页的路径也相对于当前站点，格式为`.html`，上例中`/master/form.html`为母版文件。母版页支持传递参数，上例中问号后面的部分为参数，格式同 URL 传参一样。参数会自动替换母版页中的参数占位符。详细信息见 [Voyager 母版页](/voyager/master.md)。

## 逻辑前置 Setup

前置逻辑可以让一段服务器端代码优先执行。在不使用母版页时，把服务器端代码放在页面最前面即可，但如果使用了母版页，我们又希望于有服务器端逻辑首先被执行，从而在母版页中可以使用这段代码的一些结果，比如变量和数据。一般用于使用了母版页时。

```html
<#page template="/master/form.html"/>
@{
    "name": "Tom",
    "age": 18
}

<%!
VAR $student := SELECT * FROM students WHERE id=#{id} -> FIRST ROW;
%>
```

上例中，由`<%!`和`%>`包含的部分即“逻辑前置”，这段代码会在页面请求时被首先执行，然后在母版页中可以使用变量`$student`。

## 用`#`和`#`包围的多语言标签

先看示例

```html
<div># title #</div>
<div><%= "# name #" TO UPPER %></div>
```

其中的`title`和`name`是预先在配置好的语言标签，详见[Voyager多语言支持](/voyager/language.md)。


---
参考链接

* [Voyager 多语言支持](/voyager/language.md)
* [Voyager 母版页](/voyager/master.md)
* [Voyager 参数处理](/voyager/query.md)