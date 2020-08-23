# 跨数据源数据流转 SAVE
将数据从一个数据源流转到另一个数据源是数据计算中最常见的场景，PQL提供了一个组合来处理这个问题。示例如下：
```sql
OPEN mysql.classes;
    GET # SELECT name, score FROM students;
SAVE TO mysql.school;
    PUT # INSERT INTO scores (name, score) VALUES (?, ?);
```
### 以上示例中，各语句的功能解释如下：
* OPEN语句用来打开源数据库。
* GET语句从源数据库获取数据并将数据保存在缓冲区中。
* SAVE语句用来指定目标数据库。
* PUT语句将缓存中的数据提交到目标数据库。  

### 一些需要注意的问题：
* SAVE语句默认指向DEFAUT数据源，即当前PQL中未使用过SAVE语句，则PUT会将数据保存到默认数据源。
* OPEN和SAVE可以是同一个数据源。
* PUT语句使用批量更新，每1000条提交一次，性能很高。
* PUT语句中的数据项占位符（示例中是问号“?”）的更多说明，见[PUT语句](/pql/put.md)。
* 中间缓存区受JVM限制，不建议保存太多的数据，一般经验是20000行。如果远大于这个数字，PQL还提供了其他办法来流转数据，见[大数据量下的分页和分块读取 PAGE/BLOCK](/pql/page.md)。
* 源数据库和目标数据库可以是不类型的数据库，SQL语句的语法由数据库类型确定，PQL会将SQL语句提交到指定的数据库执行。
* SAVE后面也支持AS关键词，对于关系型数据库来说，AS和TO的意义相同。
  
### 下面是一个比较复杂的示例：
```sql
OPEN mysql.db1;
    GET # SELECT name, score FROM tabel1;
    GET # SELECT name, score FROM table2;
OPEN mysql.db2;
    GET # SELECT name, score FROM table3;
SAVE TO mysql.db3;
    PREP # DELETE FROM table4;
    PUT # INSERT INTO table4 (name, score) VALUES (?, ?);
SAVE TO mysql.db4;
    PUT # UPDATE table5 SET score=#score WHERE name='#name';
    PUT # DELETE FROM table6 WHERE 
```
* 可以从一个数据源多次GET数据。
* 可以从不同的数据源GET数据。
* 多次GET得到的数据会`UNION ALL`成一个表。
* 可以将GET到数据PUT多次，每次都会PUT所有GET到的数据。
* 可以PUT数据到不同的数据源。
* PREP语句用于在PUT前在目标数据源执行一次非查询操作，详见[PREP语句](/pql/prep.md)。
* 缓存中的数据不需要显式清除，只要经过一次PUT之后，再次GET时会清空缓冲区中的数据。（非常重要）
* GET语句的更多功能，详见[GET语句](/pql/prep.md)。
* PUT语句的更多功能，详见[PUT语句](/pql/put.md)。
* 可以将GET到的数据再次进行查询而非更新，详见[PASS语句](/pql/pass.md)
* 大数据量下，PUT语句性能会受到限制，PQL提供了多线程下PUT的方法，请参见[BATCH语句](/pql/batch.md)
* SAVE语句还有更多的功能，如[保存Excel](/pql/excel.md)等，详见相应的功能模块。
    
---
参考链接
* [PQL數据源配置](/pql/properties.md)
* [打开和切换数据源 OPEN](/pql/open.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [在目标数据源执行非查询操作 PREP](/pql/prep.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)