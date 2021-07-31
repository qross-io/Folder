# 变量声明 VAR 语句

除了 SET 语句外，VAR 语句也用来声明变量，也可以为变量赋值，作为 SET 语句的补充，可以提供 SET 语句实现不了的功能。VAR 的赋值符号也使用`:=`。

```sql
VAR $a;
VAR $a, $b := 1, $c := 2;
VAR $table := SELECT * FROM table1;
VAR $row := SELECT * FROM table1 -> LAST ROW;
VAR $array := SELECT name FROM table1 -> FIRST COLUMN;
```

上例中，第一行和第二行`$a`值都为`NULL`，第三行变量`$table`的值是一个数据表，第四行变量`$row`的值是一个数据行，最后一行变量`$array`的值是一个数组。VAR 可以连续声明和赋值多个变量，通过逗号分隔即可，如第二行。

SET 和 VAR 的区别如下：

* SET 支持析构赋值，VAR 不支持。
* VAR 可以只声明变量但不赋值，SET 不可以。
* VAR 可以赋值复合类型的变量，SET 也可以通过改变赋值符号`=:`实现。
* VAR 可以通过逗号分隔连续声明和赋值多个变量，SET 只能通过析构方式赋值和声明多个变量。
* 一般的作法是声明变量用 VAR 和 SET，更新变量用 SET。

再看一个例子

```sql
SET $a, $b, $c := [1, 2, 3]; -- 结果 $a = 1, $b = 2, $c = 3;
VAR $a, $b, $c := [1, 2, 3]; -- 结果 $a = null, $b = null, $c = [1, 2, 3];
```

---
参考链接

* [数据类型](/pql/datatype.md)
* [变量声明 SET](/pql/set.md)
* [循环控制 FOR](/pql/for.md)