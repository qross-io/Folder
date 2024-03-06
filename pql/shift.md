# 对缓冲区的数据进行简单编辑 SHIFT

[GET 语句](/pql/get.md)可以把数据保存到缓存区，使用 [TRANS 语句](/pql/trans.md)可以对缓冲区的数据进行转换，如果再对缓冲区的数据进行编辑的话，可以用 SHIFT 语句，SHIFT 语句后面跟的 [Sharp 表达式](/pql/sharp.md)，可以用的操作见[数据表相关操作](/pql/sharp-table.md)。

```sql
GET # SELECT course1, course2, course3 FROM table1;
PUT # ......
SHIFT # DELETE 'course3 >= 60' SELECT course1, course2;
PUT # ......
```

上例 SHIFT 语句在第一次 PUT 之后删除缓存中符合条件的数据并选择前两列（仅示例，本身上 SELECT 操作意义不大），再进行第二次 PUT。

---
参考链接

* [Sharp 表达式 - 数据表操作](/pql/sharp-table.md)
* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)
* [全局变量 @](/pql/global-variable.md)