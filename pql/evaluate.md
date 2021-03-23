# PQL中有返回值的语句

PQL 中有一类语句，在执行之后会返回结果，这些结果可以用来赋值或者其他操作。有返回值的语句是 PQL 的核心，可以说其他语句基本为这些语句服务的。这些有返回值的语句包括：

* 所有数据库的查询语句，主要是 SELECT 语句、SHOW 语句等，返回一个数据表。
* 所有数据库的非查询语句，主要是 INSERT、UPDATE、DELETE 等，返回数据库受影响的行数。
* [REDIS 命令语句](/pql/redis.md)，返回值依照 REDIS 命令确定。
* [PARSE 语句](/pql/parse.md)，返回值依照 PARSE 语句的参数确定，默认返回一个数据表。
* [IF](/pql/if.md) 和 [CASE](/pql/case/md) 短表达式，返回值依照条件的分支确定。
* [FILE 语句](/pql/file.md)和 [DIR 语句](/pql/dir.md)，返回值依照语句命令的类型确定。
* [EXEC 语句](/pql/exec.md)，返回值依照执行的字符串语句类型确定。
* [RUN 语句](/pql/run.md)，返回一个数据行。
* [SEND 语句](/pql/send.md)，返回一个数据行。
* [INVOKE 语句](/pql/invoke.md)，返回值根据 Java 类的方法或属性确定。

这些有返回值的语句应用比较灵活，可以用在很多地方，下面分别说明。

## 将结果赋值给变量

最常用的就是在 [SET 语句](/pql/set.md)或 [VAR 语句](/pql/var.md)中赋值给变量。

```sql
VAR $table := SELECT * FROM scores;
SET $score := REDIS GET 'Tom';
VAR $students := 
    IF @role == 'monitor' THEN
        (SELECT * FROM students WHERE score>=60)
    ELSE
        (SELECT * FROM students)
    END;
```

## 在数据流转中使用

这些有返回值的语句可以放在 [GET 前缀语句](/pql/get.md)中（除非查询语句），将获取的数据保存在缓冲区，然后可能通过 [PUT 语句](/pql/put.md)保存到数据源或进行其他操作。

```sql
GET # REDIS HGETALL students;
PUT # ....
```

更多信息请参阅 [PQL 中的数据流转](/pql/dataflow.md)。

## 作为PQL过程的结果输出

这些语句的结果可以作为整个 PQL 过程的返回值，可以用到 [OUTPUT 语句](/pql/output.md)或 [RETURN 语句](/pql/return.md)中显式输出，也可直接运行作为隐式输出。

```sql
OUTPUT # REDIS KEY *; -- 显示输出
SELECT * FROM students; -- 隐式输出
```

关于显式输出和隐式输出的更多解释，请参见 [OUTPUT 语句](/pql/output.md)中的详细说明。

## 在条件判断中使用

有返回值的语句可以直接用在条件判断中，注意语句需要用小括号包围。

```sql
IF EXISTS (SELECT id FROM table1) THEN ...
IF $id IN (SELECT id FROM table1) THEN ...
```

更多信息，请参阅[条件表达式](/pql/condition.md)

## 遍历语句的结果

大部分情况下，有返回值的语句都返回一个集合类型，可以用 [FOR 语句](/pql/for.md)对这些集合进行遍历。

```sql
FOR $name, $score IN (SELECT name, score FROM students) LOOP ...
FOR $key IN (REDIS KEYS *) LOOP ...
```

## 使用 Sharp 表达式对结果再加工

因为有返回值，所以 PQL 中支持在这些语句后面写[Sharp表达式](/pql/sharp.md)对些语句的结果进行再加工，注意不能省略`->`符号。

```sql
SELECT * FROM students -> INSERT IF EMPTY (name, score) VALUES ('Tom', 89);
FOR $line IN (RUN SHELL 'ls' -> GET 'logs') LOOP ...
```

## 作为表达式嵌入其他语句

有返回值的语句可以作为查询表达式嵌入到其他语句中。

```sql
OUTPUT # {
    "exit-code": ${{ RUN SHELL java -jar /usr/home/test.jar -> GET 'code' }},
    "files": ${{ FILE LIST '/usr/home/logs/' -> FIRST COLUMN }}
}
```

相关嵌入规则和注意事项参见[嵌入式查询表达式](/pql/query.md)。


---
参考链接

* [PQL 中的 SQL 语句](/pql/sql.md)
* [用户变量 $](/pql/variable.md)
* [PQL 中的数据流转](/pql/dataflow.md)
* [变量赋值 SET](/pql/set.md)
* [变量赋值 VAR](/pql/var.md)
* [条件表达式](/pql/condition.md)
* [循环遍历 FOR](/pql/for.md)
* [对数据进行加工 Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md)