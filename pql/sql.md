# PQL中的SQL语句
编写PQL目的是为了封装整个数据处理过程，让开发人员更专注于业务逻辑，尽可能的少编写与数据处理无关的代码。所以，数据处理过程中的 **SQL** 语句仍然是PQL的核心，而PQL主要功能是把数据处理中的SQL语句组织起来。
```sql
OPEN DEFAULT;
    UPDATE students SET score=89 WHERE name='Tom';
    GET # SELECT 'A' AS rate, COUNT(0) AS amount FROM students WHERE score>=90;
    GET # SELECT 'B' AS rate, COUNT(0) AS amount FROM students WHERE score>=75 AND score<90;
    PUT # INSERT INTO scores (rate, amount) VALUES (?, ?);  
```
我们来看一下上面的示例做了哪些工作。

* 打开默认数据源。
* 更新了一行数据。
* 查询了两次数据。
* 将上面两次查询的结果保存以数据表。  

上面示例中，只有`OPEN`语句、两个`GET #`和一个`PUT #`是PQL的元素，其他的都是原生SQL语句。PQL就是通过这些附加在SQL之上的元素将多个SQL语句组织起来。这些元素书写简单，但功能强大。  

PQL对原生的SQL语句不做任何解析，会将恢复后（有时会在SQL中嵌入一些内容，如变量）提交到对应的数据源执行，再将结果保存输出。在PQL中，可以在SQL中嵌入的常用元素如下：

* 变量，包含[用户变量](/pql/variable.md)和[全局变量](/pql/global-variable.md)
* 函数，包含[用户函数](/pql/function.md)和[全局函数](/pql/global-function.md)
* [富字符串](/pql/rich.md)
* [Sharp表达式](/pql/sharp.md)
* [查询表达式](/pql/query.md)

详见见附录：[完整的嵌入规则表](/pql/place.md)，也会在对应章节进行详细介绍。

在PQL中，SQL语句被分成两类，一类是查询语句，比如`SELECT`和`SHOW`，执行结果一个二维表格; 另一类是非查询语句，比如`INSERT`、`UPDATE`、`DELETE`等，执行结果是一个整数，表示数据库受影响的行数。SQL执行的结果会被保存在全局变量或者缓冲区中，以供再计算或者输出。涉及到的全局变量如下：

* `@COUNT_OF_LAST_SELECT` 最后一次SELECT语句的结果集数量，简写为`@ROWS`。
* `@AFFECTED_OF_LAST_NON_QUERY` 最后一次非查询语句影响数据库的行数，简写为`@AFFECTED_ROWS`。
其他全局变量见对应章节。

PQL虽然不对SQL语句进行解析，但是会通过SQL语句的第一个单词来确定语句的类型，以确定执行什么类型的操作。在某些极个别情况下，PQL也无法判断SQL语句是查询语句还是非查询语句。如Hive的SELECT查询语句可以以`FROM`开头，阿里AnalyticDB有时以`WITH`开头（子查询），甚至以 /* .. */开头（用于设置参数）。PQL提供了一种简单的方法解决问题，就是在语句前加类型前缀，例如：
```sql
SELECT # WITH temp_table AS (
    SELECT name, score FROM students;
) SELECT name, score FROM temp_table WHERE score<60;
GET # SELECT # FROM table1 FROM table2 SELECT * WHERE ....;
```
是什么类型的语句就加什么前缀就好，其中的井号`#`不能省略。  

无论是查询语句还是非查询语句都属于“有返回值的语句”，这类语句有很多其他应用方式，详见[PQL中有返回值的语句](/pql/evaluate.md)。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [全局变量 @](/pql/global-variable.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql.md/)
* [更优雅的数据操作方法 Shap表达式](/pql.md/)