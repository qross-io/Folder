# 大数据量场景下的分页查询 PAGE 语句

在大数据量场景下，由于内存限制，内存中不能存储太多的数据。但是又有大数据量的流转需求。PQL 提供了一系列的语句，来实现分页或分块读取以及分块写入。这些语句在多线程模式下运行，会尽可能压榨机器资源以提高性能。

PAGE 语句是 [GET 语句](/pql/get.md)的一个高级版本，适用于大数据量下的分页读取，并通过 [PUT 语句](/pql/put.md)或 [BATCH 语句](/pql/batch.md)将数据保存到目标数据源。这个操作是一个持续的过程，一边读一边写，直到所有数据读取并写入完成。PAGE 语句仅支持关系数据库的 SELECT 查询。

```sql
OPEN mysql.classes;
    PAGE # SELECT name, score FROM table1 LIMIT @{offset}, 10000;
SAVE TO mysql.school;
    PUT # INSERT INTO (name, score);
```

上例可以理解为是一个多线程程读加单线程写操作（多线程写见 [BATCH 语句](/pql/batch.md)），使用 SQL 语句的`LIMIT`进行分页读取，其中占位符`@{offset}`固定，不可修改。这种方式的缺点是越到后面查询越慢，与数据库本身的机制有关。

```sql
OPEN mysql.classes;
    PAGE # SELECT * FROM table1 WHERE id>@{id} LIMIT 10000;
SAVE TO mysql.school;
    PUT # INSERT INTO (name, score);
```

上例是 PAGE 语句支持的第二种格式，按主键位置进行查询，占位符`@{id}`的名称必须与主键名称一致。这种方式的效率基本是固定的，但是不在多线程下运行。适用于数据量不是特别大的场景。

因为 PAGE 语句是多次查询且数据不进缓冲区，所有与 GET 语句的全局变量有一点点不同。

* `@COUNT_OF_LAST_GET` 这个不建议使用，获取值为 PAGE 最后一次查询的结果数量。
* `@TOTAL_COUNT_OF_RECENT_GET` 这个是 PAGE 所有查询的结果总数。

如果你要查询的表有自增主键，期望一个查询稳定且多线程版本操作，请参阅 [BLOCK 语句](/pql/block.md)。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)