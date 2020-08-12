# 完整的嵌入规则表
在PQL中，有非常多的嵌入符号，用于将PQL元素嵌入到原生SQL语句或其他语句中。各种嵌入在相关章节已有介绍，现在将各嵌入规则汇总一下。

除[ECHO语句](/doc/pql/echo)以外，其他语句的嵌入规则和解析顺序如下：
1. **PQL、OoneApi接口或Voyager模板引擎传递的参数**  
   格式 `#{name}`  
   防止SQL注入的格式 `#{'name'}` 或 `#{"name"}`
2. **模板引擎服务器端语句符号**  
   输出单个值格式 `<%= value %>`  
   输出单个语句或表达式`<%= expression %>`  
   多行语句或逻辑  
   ```
   <%
   sentence;
   statements...
   %>
   ```
   详见[Voyager模板引擎](/doc/voyager/overview)。
3. **用户变量**  
   格式 `$name` 或 `$(name)`
4. **全局变量**  
   格式 `@name` 或 `@(name)`
5. **Sharp表达式**  
   格式 `${ expression -> link }` 或 `${ sentence -> link }`，其中`link`可选。
6. **查询表达式**  
   格式 `${{ sentence -> link }}` Sharp表达式的高级形式，其中`link`可选。
7. **忽略传值的数据类型符号**  
   写在嵌入的变量或表达式之后，是一个叹号`!`，如`$name!`、`@name!`、`${expression}!`、`${{sentence}}!`。
8. **[PAGE语句](/doc/pql/page)和[BLOCK语句](/doc/pql/block)中的分页或分块参数**  
   格式`@{offset`}或`@{id}`
9. **JDBC原生查询占位符**  
   格式为一个问号`?`，如`UPDATE table1 SET name=? WHERE id=?`，需按数量和顺序进行传值。
10. **[PASS语句](/doc/pql/pass)/[PUT语句](/doc/pql/put)/[PROCESS语句](/doc/pql/process)/[BATCH语句](/doc/pql/batch) 中的传值符号**  
   格式`#name`或`#{name}`，不判断数据类型 
   格式`&name`或`&(name)`，判断数据类型决定是否加引号

[ECHO语句]的占位符及解决顺序如下：
1. **多语言符号**  
   格式为 `# holder #`，参见[Voyager多语言支持](/doc/voyager/language)
2. **嵌入式变量**  
   格式为`${name}`或`@{name}`，替换时不保留引号。
   

上述占位符中，比较常用的是`3`、`5`和`10`。

---
参考链接
* [向PQL传递参数](/doc/pql/params)
* [简单输出 ECHO](/doc/pql/echo)
* [用户变量 $](/doc/pql/variable)
* [全局变量 @](/doc/pql/global)
* [Sharp表达式](/doc/pql/sharp)
* [嵌入式查询表达式](/doc/pql/query)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [缓冲区数据再查询 PASS](/doc/pql/pass)
* [大数据量下的分页查询 PAGE](/doc/pql/page)
* [大数据量下的分块查询 BLOCK](/doc/pql/block)
* [大数据量下的批量更新 BATCH](/doc/pql/batch)
* [大数据量下的数据加工 PROCESS](/doc/pql/process)