# 返回结果并中断执行 RETURN

RETURN 语句和 [OUTPUT 语句](/pql/output.md)一样，都可以返回 PQL 过程的数据，但是又有一些区别。

```sql
RETURN $x(4);

FUNCTION $x($a, $b DEFAULT 3)
    BEGIN
        RETURN $a * $b;
    END;

```

* RETURN 语句和 [OUTPUT 语句](/pql/output.md)语法基本相同，相同的地方不再赘述。
* RETURN 语句可以在函数中使用，用做函数的返回值，但 OUTPUT 不行。
* OUTPUT 不能用在函数体中，否则调用一次函数就会向结果列表增加一个数据结果项。
* RETURN 返回数据的同时会中断执行，在函数中时会跳出函数，在整个 PQL 过程中时，会中断整个程序。但是 OUTPUT 不会，OUTPUT 可以写在 PQL 过程的任何地方。
* OUTPUT 可以使用多次，输出一个结果项列表。但是 RETURN 只能使用一次。
* RETURN 关键字后面可以为空，如`RETURN;`。这时仅中断过程执行，返回值为`null`。
* 可以把 OUTPUT 和 RETURN 混合使用，如使用多次 OUTPUT 最后使用 RETURN。
* 建议在函数体中使用 RETURN，在整个 PQL 过程中使用 OUTPUT。

---
参考链接

* [简单输出 ECHO](/pql/echo.md)
* [数据输出 OUTPUT](/pql/output.md)
* [数据类型](/pql/datatype.md)
* [模板引擎 Voyager](/voyager/overview.md)