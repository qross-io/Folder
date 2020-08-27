# 基本数据库操作 OPEN
在PQL中，可以在配置文件中配置多个数据源连接，作者曾经所在的数据部门，需要在上百个不同的数据源之间进行数据流转操作。可以通过OPEN语句进行数据源的连接和切换操作。
```sql
OPEN DEFAULT;
```
或
```sql
OPEN 'hive.prime';
```
默认数据源是默认连接的，但在切换数据源时仍需要使用OPEN语句。  
在OPEN语句中，连接名的引号可以省略。
打开数据源后，可以对数据库进行操作，直接写数据源对应的SQL语句即可。请参见[PQL中的SQL语句](/pql/sql.md)
```sql
OPEN mysql.classes;
    UPDATE table1 SET update_time=create_time WHERE id=1;
    SELECT * FROM table1 WHERE id=1;
OPEN mysql.school;
    INSERT INTO table2 (name, score) VALUES ('Tom', 89); 
```
以上示例中，UPDATE和SELECT语句是在`mysql.classes`数据库中执行的，而INSERT语句是在`mysql.school`数据中执行的。在开发过程中，可以通过OPEN语句，切换任意一个已经配置好的数据源。几点说明：

* 同一时间只能对某一个数据源进行操作，只要未使用另外一个OPEN语句，则下面的所有SQL操作都针对这个数据源。
* 数据连接打开后暂存在当前PQL过程中，再次使用OPEN语句时不会重复连接。
* 数据源不需要显式关闭，即没有CLOSE语句，当整个PQL执行完成之后，会自动关闭所有已经打开的数据源。
* 在数据处理过程中，直接的SELECT语句没有太大意义，但经常用在接口开发中，用来返回数据。

OPEN语句在使用时还可以切换默认数据库。
```sql
OPEN mysql.school USE 'grade3';
```
上例表示打开数据的同时将默认数据库切换到`grade3`，这个场景用得很少，一般会在SQL里指定数据库名。

OPEN语句在[跨数据流转](/pql/dataflow.md)中经常用到，由`OPEN+GET+SAVE+PUT`构成一个超强组合。见[SAVE语句](/pql/save.md)。  
OPEN语句还有其他用途，如打开Redis或者以数据表的形式打开一个文件，将在对应的章节进行介绍。

---
参考链接

* [PQL數据源配置](/pql/properties.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [PQL中的SQL语句](/pql/sql.md) 