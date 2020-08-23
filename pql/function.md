# 自定义函数 FUNCTION语句
有开发过程中，有时需要定义一个可重复调用的语句块，以减少代码量且可以让代码逻辑清晰。这个语句块可以使用PQL用户函数来定义，使用FUNCTION语句进行函数声明。
```sql
FUNCTION $func_name ($a, $b DEFAULT 'hello')
     BEGIN
           -- statements …
           RETURN $a + $b;
     END;
```
* `$func_name`是函数名。和用户变量一样，用户函数名必须以`$`开头，由英文字母、数字和下划线构成。
* 函数的作用域仅是当前PQL过程，可以在当前过程的任何地方定义。
* `$a`和`$b`是函数的参数。函数参数声明也必须以`$`开头，作用域仅为当前函数内。
* 函数参数可以使用`DEFAULT`关键词赋默认值。
* 所有函数参数使用小括号括起来。
* 可以不声明任何参数，但也要输入一个空括号。
* 不需要声明函数的返回值。
* 函数体以`BEGIN`关键字开始，并以`END`关键字结束。
* `END`关键字后面的分号不能省略，表示FUNCTION语句的结束。
* 在函数中使用`RETURN`语句返回结果，可以是PQL支持的数据类型。参见[RETURN语句](/pql/return.md)。
* RETURN语句仅在函数体内使用，RETURN语句返回函数的结果并中断函数的执行。
* 函数可以没有RETURN语句，就是可以没有返回值。没有RETURN语句的函数返回`NULL`。
* 函数可以嵌套函数，如`$x(2, $p(3, 4))`。

定义完成的函数如何调用，请参阅[CALL语句](/pql/call.md)。
如果想定义作用于所有PQL过程的函数，请参照[全局函数](/pql/global-function.md)


---
参考链接
* [函数调用 CALL](/pql/call.md)
* [中断数据并返回值 RETURN](/pql/return.md)
* [全局函数 FUNCTION](/pql/global-function.md)