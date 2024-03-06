# 查询表达式

在接口编写过程中，为了配合前端工程师的需要，经常需要返回复杂的数据结构。

```sql
OUTPUT {
    "projects": ${{ SELECT * FROM projects }},
    "jobs": ${{ SELECT id FROM jobs -> FIRST COLUMN }}
};
```

上例中由`${{`和`}}`包围的部分就是嵌入式查询表达式，可以将查询表达式理解为是 [Sharp 表达式](/pql/sharp.md)的高级一点的版本。查询表达式必须有返回值，即只能嵌入[有返回值的语句](/pql/evaluate.md)，没有返回值有可能会发生异常。

查询表达式只用于嵌入到其他语句中，目的是让代码更优雅一些。上例中可以改写为

```sql
VAR $projects := SELECT * FROM projects;
VAR $jobs := SELECT * FROM jobs -> FIRST COLUMN;
OUTPUT {  "projects": $projects, "jobs": $jobs };
```

本身 Sharp 表达式中也支持写语句，上例中把`${{`和`}}`修改成`${`和`}`也不会有什么错误，执行结果是一样的。查询表达式和 Sharp 表达式的区别：

* 查询表达式可以再嵌入 Sharp 表达式，而 Sharp 表达式不能再嵌入 Sharp 表达式。
* 对于复杂的数据语句或 Json 结构中，`${{`和`}}`比`${`和`}`更容易正确识别。

除以上两点外，查询表达式和 Sharp 表达式可以通用，表达式的内容一样，执行结果也一样。

---
参考链接

* [PQL 中有返回值的语句](/pql/evaluate.md)
* [数据输出 OUTPUT 语句](/pql/output.md)
* [PQL 中的 SQL 语句](/pql/sql.md)
* [更优雅的数据处理方法 -- Sharp 表达式](/pql/sharp.md)