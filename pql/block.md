# 大数据量场景下的分块查询 BLOCK语句
使用[PAGE语句](/doc/pql/page)进行分页读取数据的问题就是不能保证持续稳定的性能，在数据表有主键的情况下，可以使用BLOCK语句进行分块查询。
```
SET $start := SELECT MIN(id) FROM table1;
SET $end := SELECT MAX(id) FROM table1;
BLOCK FROM $start TO $end PER 10000 # SELECT name, score FROM table1 WHERE id>@{id} AND id<=@{id};
BACTH # INSERT INTO table2 (name, score) VALUES (?, ?);
```
BLOCK分块读取解决了[PAGE语句](/doc/pql/page)越查越慢的问题，需注意几点问题：
* 需要事先计算并在BLOCK语句中指定主键的起点和终点，即`FROM`和`TO`。
* `PER`表示每次查询数据的最大数量，每次查询的具体数量由主键是否连续决定。如果不指定，则默认为`10000`行每次。
* 上面占位符`@{id}`中的名称必须与数据表中自增主键名的一致。
* 条件式中的比较符号`>`和`<=`可以修改成`>=`和`<`，但不能同时都有`=`号。
* BLOCK语句也仅支持关系型数据库下的SELECT查询。

[PAGE语句](/doc/pql/page)和BLOCK语句都将每次查询的数据保存在缓冲区，当保存或更新到目标数据库后即将这次查询的数据删除，以保证内存不被数据撑爆。所以在数据流转完成时，缓冲区的数据是空的。注意这种情况下不能再通过全局变量`@BUFFER`访问缓冲区的数据。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [大数据量下的分页查询 PAGE](/doc/pql/page)
* [大数据量下的批量更新 BATCH](/doc/pql/batch)