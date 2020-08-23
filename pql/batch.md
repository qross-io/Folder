# 大数据量下的批量更新 BATCH语句
PQL提供了[GET语句](/pql/get.md)的多线程版本[PAGE语句](/pql/page.md)和[BLOCK语句](/pql/block.md)，也提供了[PASS语句](/pql/pass.md)的多线程版本[PROCESS语句](/pql/process.md)。同样，PQL也提供了[PUT语句](/pql/put.md)的多线程版本BATCH语句，用于在大数据量场景下的数据流转过程中保存或更新数据。
```sql
OPEN mysql.classes;
    PAGE # SELECT name, score FROM table1 LIMIT @{offset}, 10000;
SAVE TO mysql.school;
    BATCH # INSERT INTO table2 (name, score);
```
再看一下[PROCESS语句](/pql/process.md)中的例子：
```sql
OPEN mysql.db1;
    PAGE # SELECT id FROM table1 LIMIT @{offset}, 10000;
OPEN mysql.db2;
    PROCESS # SELECT id, title FROM table2 WHERE id=#id;
SAVE TO mysql.db3;
    BATCH # INSERT INTO table3 (id, title);
```
几点说明：
* BATCH语句只接收PAGE语句、BLOCK语句和PROCESS语句查询的数据。
* 同样的，BATCH语句可以和其他语句是相同的数据源也可以是不同的数据源。
* BATCH语句中仅支持关系型数据库的非查询语句。
* BATCH语句也支持和[PUT语句](/pql/put.md)一样的INSERT串接。


### PQL中的数据流转模式总结
GET、PUT、PAGE、BATCH等各个数据流转语句构成了一个标准的生产者消费者数据流。各个语句可以进行各种组合运用，以适应不同的场景。合理利用不同的组合，让数据开发可以事半功倍，以下是可用的组合模式。

* `GET + PUT`  单线程生产+单线程消费
* `GET + PASS + PUT` 单线程生产+单线程加工+单线程消费
* `PAGE/BLOCK + PUT` 多线程生产+单线程消费
* `PAGE/BLOCK + PROCESS + PUT` 多线程生产+多线程加工+单线程消费
* `PAGE/BLOCK + BATCH` 多线程生产+多线程消费
* `PAGE/BLOCK + PROCESS + BATCH` 多线程生产+多线程加工+多线程消费

---
参考链接
* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [使用缓冲区数据再查询 PASS](/pql/pass.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的数据加工 PROCESS](/pql/process.md)