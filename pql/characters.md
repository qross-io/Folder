# PQL特殊字符表
PQL使用了很多符号用来实现各种功能，这些符号可能与业务语句中的符号冲突，这时可以使用符号对应的编码代替。解释器在执行时会将这些特殊编码恢复成对应的字符。

| **字符** | **编码** |
| :----: | :----: |
| ; | `~u003b` |
| $ | `~u0024` |
| @ | `~u0040` |
| ! | `~u0021` |
| - | `~u002d` |
| / | `~u002f` |
| ( | `~u0028` |
| ) | `~u0029` |
| \r回车 | `~u000d` |
| \n换行 | `~u000a` |
| # | `~u0023` |
| & | `~u0026` |

举个在[PQL注释](/pql/comment.md)中介绍的例子。
AnalyticDB在查询时有时需要设置参数，比如Hive也经常需要设置参数。
```sql
OPEN `adb`;
    GET # /*+nested_loop_join=true*/ SELECT * FROM table1;
```
以上语句虽然可以正常工作，但是PQL会把`/*+nested_loop_join=true*/`部分当作注释去掉再执行，就像没有设置参数一样，执行性能会达不到预期。上例可修改为：
```sql
OPEN `adb`;
    GET # SELECT # ~u002f*+nested_loop_join=true*~u002f SELECT * FROM table1;
```
把`/`转成`/`，这样PQL就不会把这部分当作注释了。加`SELECT #`的原因是因为这条语句不是以`SELECT`开头了，需要告诉解析器这是一条查询语句，当作SELECT语句来执行，参见[PQL中的SQL语句](/pql/sql.md)。


---
参考链接
* [PQL中的注释](/pql/comment.md)
* [PQL中的SQL语句](/pql/sql.md)
* [打开和切换数据库 OPEN](/pql/open.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)