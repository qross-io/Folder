# 运行Shell命令 RUN

RUN语句用来运行Shell命令，属于PQL跟其他语言交互的一部分。示例：
```sql
SET $ls := RUN SHELL "ls";
FOR $line IN $ls.logs LOOP
    PRINT $line;
END LOOP;
```

RUN语句功能比较简单，运行Shell命令并且返回运行日志。RUN语句的结果是一个数据行，有3个字段：

* `code` 命令的退出码，是一个整数，`0`表示正常退出，其他表示异常。
* `logs` 命令的控制台的所有输出日志（包含错误日志），是一个数组，数组的项就是日志的每一行。每行日志也会打印在PQL过程的控制台上。
* `errors` 命令的控制台错误日志，也是一个数组。

RUN语句和其他[有返回值的语句](/pql/evaluate.md)一样，除可以单独执行外，可以用来赋值、通过Sharp表达式再编辑、用循环遍历日志或嵌入到其他语句中。


---
参考链接

* [PQL中有返回值的语句](/pql/evaluate.md)
* [在PQL中与Java交互](/pql/invoke.md)
* [PQL基本语法](/pql/basic.md)
* [PQL中的Javascript](/pql/javascript.md)