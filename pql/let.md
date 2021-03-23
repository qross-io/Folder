# 独立存在的 Sharp 表达式 - LET 语句

有一种场景下，需要集合变量进行编辑，而 VAR 和 SET 语句又不能满足对集合变量的编辑需求。这要使用 LET 语句解决这个问题，LET 语句中可以理解为独立存在的 Sharp 表达式。

```sql
VAR $RECENT := [];
SET $M := $C TICK BY $T;
FOR $I IN 1 TO 10 LOOP
    IF $M IS NOT NULL THEN
        LET $RECENT ADD ${ $M FORMAT 'yyyy-MM-dd HH:mm:ss' };
        SET $T := $M PLUS 1 SECOND;
        SET $M :=  $C TICK BY $T;
    END IF;
END LOOP;
```

上例是 Qross Master 中验证 Cron 表达式的接口逻辑，结果返回 Cron 表达式最近的 10 次执行时间。例子中使用一个数组`$RECENT`保存结果，每次循环都向数组中添加一项。LET 语句中还能再嵌入一个 Sharp 表达式。

```sql
LET $row SET column1=1, column2=2, column3=3  REMOVE column1;
```

上例中对数据行变量`$row`进行了两次编辑，添加了 3 个字段然后移除了 1 个字段。

---
参考链接

* [更优雅的数据操作 - Sharp表达式](/pql/sharp.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)