# 继续下一次循环 CONTINUE 语句

CONTINUE 语句用在循环中，作用是跳过当次循环后面的语句继续执行下一次循环。

```sql
FOR $i IN 1 TO 10 LOOP
    IF $i == 5 THEN
        CONTINUE;
    END IF;
    PRINT $i;
END LOOP;

SET $i := 1;
WHILE $i < 10 LOOP
    SET $i := $i + 1;
    IF $i == 5 THEN
        CONTINUE;
    END IF;
    PRINT $i;
END LOOP;
```

上例中，当变量`$i`等于`5`时，会跳过后面的语句`PRINT $i;`，继续执行下一次循环。跟 [EXIT 语句](/pql/exit.md)一样，上例还可以简写为：

```sql
FOR $i IN 1 TO 10 LOOP
    CONTINUE WHEN $i == 5;
    PRINT $i;
END LOOP;

SET $i := 1;
WHILE $i < 10 LOOP
    SET $i := $i + 1;
    CONTINUE WHEN $i == 5;
    PRINT $i;
END LOOP;
```

WHEN 关键字后使用[条件表达式](/pql/condition.md)，规则与 IF 语句和 WHILE 语句的条件表达式相同。

如果想退出整个循环体不再继续执行下一次循环，可以使用 [EXIT 语句](/pql/exit.md)。

---
参考链接

* [中断循环 CONTINUE](/pql/exit.md)
* [循环遍历 FOR](/pql/for.md)
* [条件循环 WHILE](/pql/while.md)
* [条件表达式](/pql/condition.md)