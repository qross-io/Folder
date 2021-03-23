# 大数据量下的多线程数据加工 PROCESS 语句

如果说 [PAGE 语句](/pql/page.md)和 [BLOCK 语句](/pql/block.md)是 [GET 语句](/pql/get.md)的多线程版本的话，那么 PROCESS 语句就是 [PASS 语句](/pql/pass.md)的多线程版本。PROCESS 语句的格式和 [PASS 语句](/pql/pass.md)一致。在数据处理中，[PASS 语句](/pql/pass.md)使用的非常少，所以 PROCESS 语句的使用场景更少。

```sql
OPEN mysql.db1;
    PAGE # SELECT id FROM table1 LIMIT @{offset}, 10000;
OPEN mysql.db2;
    PROCESS # SELECT id, title FROM table2 WHERE id=#id;
SAVE TO mysql.db3;
    BATCH # INSERT INTO table3 (id, title);
```

上例中，PROCESS 语句接收 [PAGE 语句](/pql/page.md)传递的数据进行一次再查询，然后把查询结果传递给 [BATCH 语句](/pql/batch.md)进行处理，三条语句构成了一个完整的“生产者->加工者->消费者”数据处理模式。

* PROCESS 语句只接受 [PAGE 语句](/pql/page.md)和 [BLOCK 语句](/pql/block.md)查询的数据。
* 三条语句可以是相同的数据源也可以是不同的数据源。
* PROCESS 语句中只支持关系型数据库下的 SELECT 查询。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [使用缓冲区数据再查询 PASS](/pql/pass.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)