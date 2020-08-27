# 返回结果并中断执行 RETURN

RETURN语句和[OUTPUT语句](/pql/output.md)一样，都可以返回PQL过程的数据，但是又有一些区别。
```sql
RETURN $x(4);

FUNCTION $x($a, $b DEFAULT 3)
    BEGIN
        RETURN $a * $b;
    END;

```

* RETURN语句和[OUTPUT语句](/pql/output.md)语法基本相同，相同的地方不再赘述。
* RETURN语句可以在函数中使用，用做函数的返回值，但OUTPUT不行。
* OUTPUT不能用在函数体中，否则调用一次函数就会向结果列表增加一个数据结果项。
* RETURN返回数据的同时会中断执行，在函数中时会跳出函数，在整个PQL过程中时，会中断整个程序。但是OUTPUT不会，OUTPUT可以写在PQL过程的任何地方。
* OUTPUT可以使用多次，输出一个结果项列表。但是RETURN只能使用一次。
* RETURN关键字后面可以为空，如`RETURN;`。这时仅中断过程执行，返回值为`null`。
* 可以把OUTPUT和RETURN混合使用，如使用多次OUTPUT最后使用RETURN。
* 建议在函数体中使用RETURN，在整个PQL过程中使用OUTPUT。

---
参考链接

* [简单输出 ECHO](/pql/echo.md)
* [数据输出 OUTPUT](/pql/output.md)
* [数据类型](/pql/datatype.md)
* [模板引擎 Voyager](/voyager/overview.md)