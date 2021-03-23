# 条件循环 WHILE 语句

WHILE 循环的逻辑相对 FOR 循环来要简单很多，在满足指定条件时循环，在不满足条件时退出循环。

```sql
SET $i := 1;
WHILE $i < 10 LOOP
    PRINT $i;
    SET $i := $i + 1;
END LOOP;
```

* WHILE 条件表达式的规则与 IF 语句相同。
* `END LOOP`后面必须有分号，表示语句结束。
* 循环体内必须是语句，且每条语句均以分号结束。
* 没有`DO-WHILE`循环，如果实在需要，可由 WHILE 语句和 EXIT 语句配合实现。

```sql
SET $i := 1;
WHILE true LOOP
    SET $i := $i + 1;
    EXIT WHEN $i >= 10;
END LOOP;
```

详见 [EXIT 语句](/pql/exit.md)和 [CONTINUE 语句](/pql/continue.md)。

---
参考链接

* [条件表达式](/pql/condition.md)
* [退出循环 EXIT](/pql/exit.md)
* [继续下一次循环 CONTINUE](/pql/continue.md)
* [循环遍历 FOR](/pql/for.md)