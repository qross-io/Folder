# 在控制台输出信息 PRINT 语句

PRINT 语句用来向控制台打印一行信息，这些信息可以在开发时调试使用，也可以被调度工具记录在日志中。

```sql
PRINT "Hello World";
PRINT $name;
PRINT (1, 2, 3);
PRINT [1, 2, 3];
PRINT { "name": $name, "age": $age, "score": 98 };
PRINT ${ 1 + 1 }; -- Sharp表达式
PRINT ${{ SELECT name, score FROM table1 }};　--查询表达式
PRINT;
```

如上例所示，理论上，PRINT 语句可以在控制台输出任何信息。PRINT 表示打印一行信息，在结束时会打印换行符。PQL 没有提供只打印不换行的方法，如果想把一行信息分段分步打印，可使用[富字符串](/pql/rich.md)或其他方式将要打印的信息先连在一起。`PRINT;` 表示打印一个空行。

PRINT 支持打印带时间戳和类型的信息。如下例：

```sql
PRINT INFO "info";
PRINT DEBUG "debug info";
PRINT WARN "warning";
PRINT ERROR "exception";
```

输出结果为：

```
2020-08-04 16:45:01 [INFO] info
2020-08-04 16:45:01 [DEBUG] debug info
2020-08-04 16:45:01 [WARN] warning
2020-08-04 16:45:01 [ERROR] exception
```

其中`ERROR`类型为异常错误信息（标准错误流），其他为正常信息（标准输出流）。

也可以打印自定义的标签类型，在 PRINT 后输出任意单词即可。

```sql
PRINT HELLO "WORLD";
```

输出结果为

```sh
2020-08-04 16:45:01 [HELLO] WORLD
```

在 PQL 开发过程中，在调试模式下会自动打印很多的调试信息，请参考 [DEBUG 语句](/pql/debug.md)。

---
参考链接

* [启用调试 DEBUG](/pql/debug.md)
* [数据类型](/pql/datatype.md)
* [Sharp 表达式](/pql/sharp.md)
* [查询表达式](/pql/query.md)
* [富字符串](/pql/rich.md)