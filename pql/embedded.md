# 嵌入式 PQL

嵌入式 PQL 是 PQL 的扩展应用，其主要思想是可以将 PQL 嵌入到任何格式的文本中，如 HTML、Markdown、Json，甚至 Python。通过嵌入式 PQL 解析器解析，得到解析后的文本结果，可以在解析之前进行传参以获得不同的结果。

## 嵌入式 PQL 语法

嵌入式 PQL 的语法非常简单，跟 Asp 和 Jsp 的规则几乎一样，在要嵌入 PQL 的文本中使用`<%`和`%>`包含 PQL 语句。

```json
<%
VAR $student := SELECT name, age, score FROM students WHERE id=#{id};
%>
{
    "name": "<%=$student.name%>",
    "age": <%=$student.age%>,
    "score": <%=$student.score%>
}
```

上例中接收参数`id`，从数据表`students`中查出明细，然后把各个字段输出到页面上，最终生成一个 student 的 Json 对象数据。上例中，PQL 代码都由`<%`和`%>`包围起来，其中`<%=`和`%>`表示输出变量的值，也可输出表达式计算后的结果，是 [OUTPUT 语句](/pql/output.md)的简单形式。嵌入式 PQL 除了这些，再没有更多其他语法。

## 嵌入式 PQL 应用

嵌入式 PQL 在整个 PQL 家族中应用主要在以下几个地方：

1. [Voyager 模板引擎](/voyager/overview.md)，提供对 HTML 和 Markdown 格式的解析支持，支持一些专有的[语法规则](/voyager/syntax.md)。
2. [Keeper 工作流](/keeper/dag.md)和[Keeper 命令模板](/keeper/command-template.md)，可以使用 Shell 和 Python 编写命令或命令模板，在 Shell 或 Python 中其中嵌入 PQL 逻辑，以实现更复杂的逻辑和功能。
3. [Keeper 执行脚本事件](/keeper/event.md)和[Keeper 自定义事件](/keeper/custom-event.md)，可以在 Shell 和 Python 编写的事件中使用嵌入式 PQL。
4. [FILE 语句](/pql/file.md)中的`VOYAGE`指令，可以传递不同的参数将 PQL 嵌入的文本文件保存为解析后的文件。比如 DataX 需要 Json 格式的文件进行配置。

## 嵌入式 PQL 传参

在不同的应用中，有稍有不同的传参形式。比如 Voyager 模板引擎支持 URL 地址传参和 Json 对象传参，Keeper 工作流和命令模板提供专门的传参输入框或预设参数，FILE 语句支持`WITH`指令传参。但无论何种方式，最多支持两种格式的参数，即查询字符串格式和 Json 对象格式。

查询字符串格式为`name=Tom&age=18&score=78`，不同的参数之间使用`&`隔开，参数名和参数值之是使用`=`隔开。如果遇到字符冲突，比如参数值中包含行号`=`或`&`，那么要替换为`%3D`和`%26`，详细请参照[地址中的特殊字符处理](/voyager/query.md)。

Json 对象格式传参时不需要对特殊字符进行转义，且支持数据类型。示例如下：

```json
{
    "name": "Tom",
    "age": 18,
    "score": 78
}
```

---
参考链接

* [Voyager 模板引擎](/voyager/overview.md)
* [Voyager 语法](/voyager/syntax.md)
* [Keeper 工作流](/keeper/dag.md)
* [Keeper 事件](/keeper/event.md)
* [Keeper 命令模板](/keeper/command-template.md)
* [Keeper 自定义事件](/keeper/custom-event.md)
* [FILE 语句](/pql/file.md)
* [Voyager 参数处理](/voyager/query.md)
