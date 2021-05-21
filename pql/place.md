# 完整的占位符和嵌入规则表

在 PQL 中，有非常多的嵌入符号，用于将 PQL 元素嵌入到原生 SQL 语句或其他语句中。各种嵌入在相关章节已有介绍，现在将各嵌入规则汇总一下。

## 通用规则

除 [ECHO 语句](/pql/echo.md)以外，其他语句的嵌入规则和解析顺序基本一致，适用于几乎所有场景。

1. **PQL、[OneApi 接口](/oneapi/overview.md)或 [Voyager 模板引擎](/voyager/overview.md)传递的参数**  

   格式 `#{name}`  
   防止SQL注入的格式 `#{'name'}` 或 `#{"name"}`

2. **[嵌入式 PQL](/pql/embedded.md) 和 [Voyager 模板引擎](/voyager/overview.md)包围符号**
   
   * 输出单个值格式 `<%= value %>`  
   * 输出单个语句或表达式`<%= expression %>`  
   * 多行语句或逻辑，每条语句之间使用分号隔开。
        ```html
        <%
        sentence;
        statements...
        %>
        ```
    * [逻辑前置](/voyager/syntax.md) `<%! staement %>`，仅在 Voyager 中主页面使用母版页时可用，可将一段逻辑提到最前面执行。
    
3. **变量**  

   包括[用户变量](/pql/variable.md)、[全局变量](/pql/global-variable.md)和[调度变量](/keeper/job-variable.md)。用户变量格式 `$name` 或 `$(name)` ，全局变量格式 `@name` 或 `@(name)`，调度变量格式 `%name` 或 `%(name)`。带括号是为了防止字符冲突。

4. **集合变量属性和索引**  

   如 `$row.field`、`$list[2]`、`$table['id'].first` 等，详见[集合类型的元素访问](/pql/collection.md)。

5. **函数**

   包括[用户函数](/pql/function.md)和[全局函数](/pql/global-function.md)，格式`$fun(arg)`或`@fun(arg1, arg2)`，多个函数参数之是使用逗号`,`隔开。

6. **Sharp 表达式**  

   格式 `${ expression -> link }` 或 `${ sentence -> link }`，其中`link`为“操作”。详见 [Sharp 表达式](/pql/sharp.md)

7. **查询表达式**  

   格式 `${{ sentence -> link }}` Sharp 表达式的高级形式，其中`link`可选。详见[查询表达式](/pql/query.md)。

8. **忽略传值类型**  

   一般用于去掉嵌入变量或表达式结果字符串两端的引号，写在嵌入的变量或表达式之后，是一个叹号`!`，如`$name!`、`@name!`、`${expression}!`、`${{sentence}}!`。

9. **[PAGE 语句](/pql/page.md)和[BLOCK 语句](/pql/block.md)中的分页或分块参数**  

   格式`@{offset`}或`@{id}`，仅用于这两条语句。

10. **JDBC 原生查询占位符**  

   格式为一个问号`?`，如`UPDATE table1 SET name=? WHERE id=?`，需按数量和顺序进行传值。一般用于 [PUT 语句](/pql/put.md)和 [BATCH 语句](/pql/batch.md)中。

11. **[PASS 语句](/pql/pass.md)/ [PUT 语句](/pql/put.md)/ [PROCESS 语句](/pql/process.md)/ [BATCH 语句](/pql/batch.md) 中的传值符号**

   * 格式`#name`或`#{name}`，不判断数据类型；
   * 格式`&name`或`&(name)`，判断数据类型决定是否加引号。

上述占位符中，比较常用的是第`3`、`5`和`10`项。

## ECHO 语句

[ECHO 语句](/pql/echo.md)的占位符及解决顺序如下：

1. **多语言符号**  

   格式为 `# holder #`，参见 [Voyager 多语言支持](/voyager/language.md)。ECHO 多语言支持在`v1.5.0`版本中不再支持。

2. **嵌入式变量**  

   格式为`${name}`或`@{name}`，替换时不保留引号。即将在未来的版本中取消支持，不建议再使用。因为 ECHO 语句主要用于嵌入式 PQL 和模板引擎，所以建议由`<%=$name%>`和`<%=@name%>`代替。

---
参考链接

* [向PQL传递参数](/pql/params.md)
* [简单输出 ECHO](/pql/echo.md)
* [用户变量 $](/pql/variable.md)
* [全局变量 @](/pql/global-variable.md)
* [Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)
* [大数据量下的数据加工 PROCESS](/pql/process.md)