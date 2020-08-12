# 条件循环 WHILE语句
WHILE循环的逻辑相对FOR循环来要简单很多，在满足指定条件时循环，在不满足条件时退出循环。
```
SET $i := 1;
WHILE $i < 10 LOOP
    PRINT $i;
    SET $i := $i + 1;
END LOOP;
```
* WHILE条件表达式的规则与IF语句相同。
* `END LOOP`后面必须有分号，表示语句结束。
* 循环体内必须是语句，且每条语句均以分号结束。
* 没有`DO-WHILE`循环，如果实在需要，可由WHILE语句和EXIT语句配合实现。
```
SET $i := 1;
WHILE true LOOP
    SET $i := $i + 1;
    EXIT WHEN $i >= 10;
END LOOP;
```
详见[EXIT语句](/doc/pql/exit)和[CONTINUE语句](/doc/pql/continue)。

---
参考链接
* [退出循环 EXIT](/doc/pql/exit)
* [继续下一次循环 CONTINUE](/doc/pql/continue)
* [循环遍历 FOR](/doc/pql/for)