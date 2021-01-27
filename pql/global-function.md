# 全局系统函数

和任何数据库的 SQL 语言一样，PQL 支持系统函数，可以像使用变量一样嵌入到语句的任何地方。所以 PQL 不仅可以通过 Sharp 表达式操作数值，还可以使用函数。

```sql
SELECT * FROM students WHERE initial=@LEFT($name, 1);
```

上例效果与下列 [Sharp 表达式操作](/pql/sharp-text.md)相同。

```sql
SELECT * FROM students WHERE initial=${ $name TAKE 1 };
```

与[全局变量](/pql/global-variable.md)一样，系统函数使用`@`开头。虽然大多数情况下功能与 [Sharp表达式](/pql/sharp.md)相同，但 PQL 仍建议使用 [Sharp 表达式](/pql/sharp.md)作为处理数据的手段。一个原因是避免混乱，因为所有原生的 SQL 都支持函数，而且名称有的会与 PQL 函数相同，区别只是差别一个`@`号；二是为了避免逻辑不清，各数据库的原生函数作用于每行数据，而 PQL 函数只作用于单值。

函数与其他嵌入式操作的优先级顺序为：**用户变量** 优先于 **系统变量** 优先于 **自定义或全局函数** 优先于 **Sharp 表达式**，所以可以在函数中把变量当作函数的参数，也可以在 Sharp 表达式运算过程中使用函数。

为了兼容不同的数据库 SQL 语言，PQL 中会有不同的函数名称来完成同一个操作。

（暂未开放）可以定义自己的系统函数，作用于整个系统。需要通过 Qross Master 管理工具定义。

目前系统支持的全局函数比较少，会随着版本更新不断补全。

* [字符串处理函数](/pql/function-text.md)
* [数字相关函数](/pql/function-numeric.md)

---
参考链接

* [自定义函数 FUNCTION](/pql/function.md)
* [函数调用 CALL](/pql/call.md)
