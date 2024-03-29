# 简单输出 ECHO 语句

ECHO 语句和 OUTPUT 语句](/pql/output.md)一样，都用来输出和返回 PQL 过程的计算结果。

```html
ECHO <h1>标题1</h1>;
ECHO <div>#{name}</div>;
ECHO <div># description #</div>;
```

* [OUTPUT 语句](/pql/output.md)支持完整的规则，后面的表达式和语句都会被计算。ECHO 语句不支持任何占位符和嵌入规则，直接返回。
* ECHO 语句后面忽略数据类型，均为字符串。
* ECHO 语句属于显示输出，结果都会被输出和返回。
* ECHO 语句会去掉`ECHO`关键字后的第一个空白字符，其他空白字符不做处理。
* ECHO 语句是[嵌入式 PQL](/pql/embedded.md) 的基础。

ECHO 语句另外一种应用场景是语句占位。

```sql
ECHO;
```

这条语句什么都不做，没有任何功能，仅是一条语句。例如可以放在 [IF 语句](/pql/if.md) 或 [FOR 语句](/pql/for.md)中。

---
参考链接

* [数据输出 OUTPUT](/pql/output.md)
* [数据输出 RETURN](/pql/return.md)
* [数据类型](/pql/datatype.md)
* [模板引擎 Voyager](/voyager/overview.md)