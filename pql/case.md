# 条件判断 CASE语句
作为IF语句的补充，PQL也提供了CASE语句用于条件判断，功能和其他SQL语句中相同。
```
SET $score := SELECT score FROM students WHERE name='Tom';
SET $rate := '';
CASE true
    WHEN $score >= 90 THEN
        SET $rate := 'A';
    WHEN $score >= 75 THEN
        SET $rate := 'B';
    WHEN $score >= 60 THEN
        SET $rate := 'C';
    ELSE
        SET $rate := 'D';
    END CASE;
```
上例中的`true`可省略，这种形态的CASE语句提供了跟IF语句一样的功能。CASE语句的比较形态如下例：
```
SET $rate := SELECT rate FROM students WHERE name='Tom';
CASE $rate
    WHEN 'A' THEN
        PRINT 'Good Job!';
    WHEN 'B' THEN
        PRINT 'Excellent!';
    WHEN 'C' THEN
        PRINT 'It looks like you study hard.';
    ELSE
        PRINT 'Are you ok?';
    END CASE;
```
几点注意事项：
* 至少有一条`WHEN`。
* `THEN`后面不加分号，当然也可以加。
* `ELSE`可以省略。
* `END CASE`后面必须加分号。
* `THEN`和`ELSE`后面必须有至少一条语句，每条语句都以分号结束。如果真的什么都没有，那么请用`ECHO;`占位。

#### CASE短语句
像IF语句一样，CASE也有条件式，规则基本与IF短语句相同。
```
SET $rate := 
    CASE 
        WHEN $score >= 90 THEN 
            'A' 
        WHEN $score >= 75 THEN
            'B'
        WHEN $score >= 60 THEN
            'C' 
        ELSE
            'D'
        END; 
```
再如：
```
UPDATE students SET score=${ CASE $rate WHEN 'A' THEN 90 WHEN 'B' THEN  75 WHEN 'C' THEN 60 ELSE 60 END } WHERE name='Tom';   
```
与CASE语句的不同点有：
* CASE短语句不能独立存在，只能作为其他语句的一部分。
* CASE短语句以`END`结尾。
* CASE短语句`THEN`或`ELSE`后面不是语句，是单个值，而且后面不能加分号。
* CASE短语句必须有返回值，即`ELSE`部分必须有。
* CASE短语句`THEN`或`ELSE`后面可以写一条语句，但必须用小括号括起来，且不能加分号。
```
VAR $scores := 
    CASE $rate 
        WHEN 'A' THEN
            (SELECT name, score FROM students WHERE score>=90)
        WHEN 'B' THEN
            (SELECT name, score FROM students WHERE score>=75)
        WHEN 'C' THEN
            (SELECT name, score FROM students WHERE score>=60)
        ELSE
            (SELECT name, score FROM students WHERE score<60)
        END -> INSERT (name, score) VALUES ('Tom', 89);
```

参考链接
* [条件表达式](/doc/pql/condition)
* [变量声明 SET](/doc/pql/set)
* [变量声明 VAR](/doc/pql/var)
* [条件判断 IF](/doc/pql/if)