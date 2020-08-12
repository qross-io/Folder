# 大数据量下的多线程数据加工 PROCESS语句
如果说[PAGE语句](/doc/pql/page)和[BLOCK语句](/doc/pql/block)是[GET语句](/doc/pql/get)的多线程版本的话，那么PROCESS语句就是[PASS语句](/doc/pql/pass)的多线程版本。PROCESS语句的格式和[PASS语句](/doc/pql/pass)一致。在数据处理中，[PASS语句](/doc/pql/pass)使用的非常少，所以PROCESS语句的使用场景更少。
```
OPEN mysql.db1;
    PAGE # SELECT id FROM table1 LIMIT @{offset}, 10000;
OPEN mysql.db2;
    PROCESS # SELECT id, title FROM table2 WHERE id=#id;
SAVE TO mysql.db3;
    BATCH # INSERT INTO table3 (id, title);
```
上例中，PROCESS语句接收[PAGE语句](/doc/pql/page)传递的数据进行一次再查询，然后把查询结果传递给[BATCH语句](/doc/pql/batch)进行处理，三条语句构成了一个完整的“生产者->加工者->消费者”数据处理模式。
* PROCESS语句只接受[PAGE语句](/doc/pql/page)和[BLOCK语句](/doc/pql/block)查询的数据。
* 三条语句可以是相同的数据源也可以是不同的数据源。
* PROCESS语句中只支持关系型数据库下的SELECT查询。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [使用缓冲区数据再查询 PASS](/doc/pql/pass)
* [大数据量下的分页查询 PAGE](/doc/pql/page)
* [大数据量下的分块查询 BLOCK](/doc/pql/block)
* [大数据量下的批量更新 BATCH](/doc/pql/batch)