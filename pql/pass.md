# 使用缓冲区数据再查询 PASS
虽然很少遇到这个场景，但这种情况确实存在。
```sql
OPEN mysql.db1;
    GET # SELECT name, score FROM table1;
OPEN mysql.db2;
    PASS # SELECT id FROM table2 WHERE name=&name AND score=#score;
SAVE AS mysql.db3;
    PUT # DELETE FROM table3 WHERE id=#id;
```
上面示例中，PASS语句把GET语句得到的数据当成参数再进行一次查询，然后把得到的数据再存在缓冲区，最后再PUT。数据经过一次传递，得到最终期望的结果，可以理解为缓冲区数据的再加工。  
使用PASS语句的几点说明：

* PASS语句支持的数据占位符与PUT语句一样，详见[PUT语句](/pql/put.md)中的说明。
* PASS语句的原理是遍历缓冲区的数据，用每一条数据当作条件进行一次再查询，然后把数据`UNION ALL`在一起。由于本身是多次查询，所以不建议在大数据量下使用PASS语句。但合理的使用PASS语句能够让计算逻辑更清晰。
* PASS语句会清空GET的数据并将自己查询的数据保存在缓冲区。
* 与GET和PUT相同，PASS语句也有自己的多线程版本，请参见[PROCESS语句](/pql/process.md)。

在PASS完成之后，有几个全局变量可用：

* `@COUNT_OF_LAST_GET` 这一次查询的总数据量（表格行数），可简写为`@COUNT`
* `@TOTAL_COUNT_OF_RECENT_GET` 这个值现在表示PASS语句和GET语句的总数量，可简写为`@TOTAL`
* `@BUFFER` 可以通过这个全局变量访问缓冲区
* `@COUNT_OF_LAST_SELECT` 这一次查询的总数量量（表格行数），可简写为`@ROWS`。同`@COUNT_OF_LAST_GET`

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的数据再加工 PROCESS](/pql/process.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)