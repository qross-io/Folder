# 运行文件和命令 RUN

RUN 语句可以用来运行 Shell 命令，属于 PQL 跟其他语言交互的一部分。示例：

```sql
VAR $ls := RUN SHELL "ls";
FOR $line IN $ls.logs LOOP
    PRINT $line;
END LOOP;
```

RUN 语句功能比较简单，运行 Shell 命令并且返回运行日志。RUN 语句的结果是一个数据行，有`3`个字段：

* `code` 命令的退出码，是一个整数，`0`表示正常退出，其他表示异常。
* `logs` 命令的控制台的所有输出日志（包含错误日志），是一个数组，数组的项就是日志的每一行。每行日志也会打印在PQL过程的控制台上。
* `errors` 命令的控制台错误日志，也是一个数组。

RUN 语句还可以用来执行 PQL 文件。

```sql
RUN PQL '/sql/test.sql?id=1';
```

PQL 文件需要完整的路径，参数可以放在命令后面，使用问题`?`隔开，参数部分可以省略。

RUN 语句和其他[有返回值的语句](/pql/evaluate.md)一样，除可以单独执行外，可以用来赋值、通过 Sharp 表达式再编辑、用循环遍历日志或嵌入到其他语句中。

另外，使用 [PAR 语句](/pql/par.md) 可以并行执行 PQL 文件或 Shell 命令。

---
参考链接

* [PQL 中有返回值的语句](/pql/evaluate.md)
* [在 PQL 中与 Java 交互](/pql/invoke.md)
* [PQL 基本语法](/pql/basic.md)
* [PQL 中的 Javascript](/pql/javascript.md)
* [多线程并行运算 PAR](/pql/par.md)