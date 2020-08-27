# 条件控制 IF语句
在数据开发过程中，控制语句用得相对比较少，因为SQL中的`WHERE`条件已经完成了大部分的工作。PQL提供了与PL/SQL一样条件判断语句IF和[CASE](/pql/case.md)，这里先看IF语句。
```sql
SET $where := '';

IF $key <> '' THEN
    SET $where := """name LIKE '%$key%'""";
ELSIF $rate == 'A' THEN
    SET $where := "score>=90";
ELSIF $rate == 'B' THEN
    SET $where := "score<90 AND score>=75";
ELSE
    SET $where := "1=1";
END IF;

SELECT * FROM students WHERE $where!;
```
上例通过条件判断进行`WHERE`条件式的拼接，最后将条件式应用到SELECT语句进行查询。几点注意事项：

* `THEN`后不加分号，虽然加分号也不会出错。
* `THEN`或`ELSE`后面要有至少一条语句，如果确实什么都没有，请输入`ECHO;`占个位置。
* `ELSIF`千万不能写成`ELSE IF`。
* `END IF`后面必须加分号，表示IF语句结束。
* `ELSE`可以有可以没有。
* `ELSIF`可以有可以没有。
* `IF`和`THEN`之间是条件判断的内容，详见[条件表达式](/pql/condition.md)。


### IF短语句
除了上述的完整的IF语句之外，IF语句还有另一种形态，称为 **IF短语句**。IF短语句不能独立存在，必须集成在语句中，比如SET语句的右侧赋值表达式，或嵌入Sharp表达式等。
```sql
SET $rate := 
    IF $score >= 90 THEN 
        'A' 
    ELSIF $score >= 75 THEN
        'B'
    ELSIF $score >= 60 THEN
        'C' 
    ELSE
        'D'
    END; 
```
再如：
```sql
UPDATE students SET rate=${ IF $score >= 90 THEN 'A' ELSIF $score >= 75 THEN 'B' ELSIF $score >= 60 THEN 'C' ELSE 'D' END } WHERE name='Tom';   
```

与IF语句的区分主要有：

* IF短语句不能独立存在，只能作为其他语句的一部分。
* IF短语句以`END`结尾。
* IF短语句`THEN`或`ELSE`后面不是语句，是单个值，而且后面不能加分号。
* IF短语句必须有返回值，即`ELSE`部分必须有。
* IF短语句`THEN`或`ELSE`后面可以写一条语句，但必须用小括号括起来，且不能加分号。
```sql
VAR $scores := 
    IF $rate == 'A' THEN
        (SELECT name, score FROM students WHERE score>=90)
    ELSE
        (SELECT name, score FROM students WHERE score<90)
    END -> INSERT (name, score) VALUES ('Tom', 89);
```
如上例，IF短语句运算完成后返回一个数据表，而且可以继续通过Sharp表达式继续运算。

参考链接

* [条件表达式](/pql/condition.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [条件控制 CASE](/pql/case.md)
* [Sharp表达式](/pql/sharp.md)