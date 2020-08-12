# 中间缓存数据库 CACHE
PQL为整个计算过程提供了两个中间数据库，这两个数据库由SQLite内存数据库实现。一个库是内存型的，另一库是文件型的。在数据计算时，可以将少量的中间数据临时存在中间数据库中，通过SQLite提供的SQL语句对数据进行再编辑。SQLite的SQL语法简单但功能强大，完全能满足数据再编辑的需求。先介绍内存数据库，这里可称为Cache数据库。
```
CACHE 'table1' # SELECT name, score FROM table1; 
```
仅通过一条CACHE语句，就可以将SELECT的查询结果保存在内存库的表`table1`中。下面是一个完整的例子，包含如何从缓存库中进行查询。
```
OPEN mysql.school;
    CACHE 'students' # SELECT name, age FROM students;
OPEN mysql.classes;
    CACHE 'scores' # SELECT name, score FROM scores;
OPEN CACHE;
    GET # SELECT A.name, A.age, B.score FROM students A INNER JOIN scores B ON A.name=B.name;
SAVE AS mysql.school;
    PUT # INSERT INTO examination (name, age, score);
```
上例中，通过CACHE语句从两个库的数据读取数据保存在中间缓存库中的表`students`和`scores`中，再通过`OPEN CACHE;`语句切换到中间缓存库进行GET操作，最后再将数据保存到`mysql.school`库中。  

#### 一些注意事项
* 中间缓存库也算作一个JDBC数据源，使用`OPEN CACHE;`语句切换后如果仍要使用其他数据源，需要再使用OPEN语句切换回去，否则接下来的查询仍会在中间缓存库中进行。
* 由于数据库在内存中，所以Cache数据库并不能支持太大的数据量，个人经验是万级。如果需要缓存大数据量进行计算，请使用[TEMP语句](/doc/pql/temp)保存数据到文件数据库。
* 如果使用CACHE语句两次且指定的表名一样时，第二次不会新建表，只是会再向缓存表插入数据，但一定保证两次指定的字段一致，至少第二次是第一次的子集。  

#### Cache数据库更多功能
* 中间缓存库也支持其他的增删改操作。
```
    OPEN CACHE;
        INSERT INTO students (name, age) VALUES ('Tom', 19);
        UPDATE scores SET score=60 WHERE name='Ted';  
```
* Cache数据库也支持SAVE和PUT的组合。
```
    SAVE TO CACHE;
        PUT # DELETE FROM students WHERE score<60; 
```
* 可通过SAVE语句为缓存表创建索引字段。使用CACHE语句创建的缓存表默认是没有主键索引的，可以通过ALTER或CREATE语句手工添加，比较麻烦。PQL提供了一种简单的方式为缓存表创建索引字段。
```
    OPEN mysql.school;
        GET # SELECT name, age FROM students;
    SAVE AS CACHE TABLE 'students' PRIMARY KEY 'id';
```
上例中通过`SAVE AS CACHE TABLE`语句创建了`students`表并创建了名为`id`的递增主键索引字段。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [中间临时文件数据库 TEMP](/doc/pql/temp)