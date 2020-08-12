# 中断循环 EXIT
EXIT语句用于中断FOR和WHILE循环。
```
FOR $i IN 1 TO 10 LOOP
    IF $i > 5 THEN
        EXIT;
    END IF;
END LOOP;

SET $i := 1;
WHILE $i < 10 LOOP
    IF $i > 5 THEN
        EXIT;
    END IF;
    SET $i := $i + 1;
END LOOP;
```
在循环中碰到`EXIT;`语句，即立刻退出循环，继续执行循环后面的语句。上例中还可以简写以下形式。
```
FOR $i IN 1 TO 10 LOOP
    EXIT WHEN $i > 5;
END LOOP;

SET $i := 1;
WHILE $i < 10 LOOP
    EXIT WHEN $i > 5;
    SET $i := $i + 1;
END LOOP;
```
`WHEN`关键字后使用[条件表达式](/doc/pql/condition)，规则与IF语句和WHILE语句中的条件表达式相同。

如果只想退出本次循环继续执行下一次循环，可以使用 [CONTINUE语句](/doc/pql/continue)。

EXIT语句还有另一个功能，就是退出整个程序，详见 [中断程序执行 EXIT CODE](/doc/pql/exit-code)。

---
参考链接
* [继续下一次循环 CONTINUE](/doc/pql/continue)
* [循环遍历 FOR](/doc/pql/for)
* [条件循环 WHILE](/doc/pql/while)
* [条件表达式](/doc/pql/condition)
* [中断程序执行 EXIT CODE](/doc/pql/exit-code)