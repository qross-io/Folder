# 大数据量场景下的分页查询 PAGE语句
在大数据量场景下，由于内存限制，内存中不能存储太多的数据。但是又有大数据量的流转需求。PQL提供了一系列的语句，来实现分页或分块读取以及分块写入。这些语句在多线程模式下运行，会尽可能压榨机器资源以提高性能。

PAGE语句是[GET语句](/doc/pql/get)的一个高级版本，适用于大数据量下的分页读取，并通过[PUT语句](/doc/pql/put)或[BATCH语句](/doc/pql/batch)将数据保存到目标数据源。这个操作是一个持续的过程，一边读一边写，直到所有数据读取并写入完成。PAGE语句仅支持关系数据库的SELECT查询。
```
OPEN mysql.classes;
    PAGE # SELECT name, score FROM table1 LIMIT @{offset}, 10000;
SAVE TO mysql.school;
    PUT # INSERT INTO (name, score);
```
上例可以理解为是一个多线程程读加单线程写操作（多线程写见[BATCH语句](/doc/pql/batch)），使用SQL语句的`LIMIT`进行分页读取，其中占位符`@{offset}`固定，不可修改。这种方式的缺点是越到后面查询越慢，与数据库本身的机制有关。
```
OPEN mysql.classes;
    PAGE # SELECT * FROM table1 WHERE id>@{id} LIMIT 10000;
SAVE TO mysql.school;
    PUT # INSERT INTO (name, score);
```
上例是PAGE语句支持的第二种格式，按主键位置进行查询，占位符`@{id}`的名称必须与主键名称一致。这种方式的效率基本是固定的，但是不在多线程下运行。适用于数据量不是特别大的场景。

如果你要查询的表有自增主键，期望一个查询稳定且多线程版本操作，请参阅[BLOCK语句](/doc/pql/block)。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [大数据量下的分块查询 BLOCK](/doc/pql/block)
* [大数据量下的批量更新 BATCH](/doc/pql/batch)