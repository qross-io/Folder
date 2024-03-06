# 对缓冲区的数据进行转换 TRANS

[GET 语句](/pql/get.md)可以把数据保存到缓存区，有时会有将缓冲区的数据进行转换的需求，比如一行转多行，使用 TRANS 语句可以对数据进行转换，这样可以避免再去数据库请求一次数据，甚至查询的 SQL 不好编写时也可以使用。

TRANS 语句接受一个 JSON 对象或对象数组，且不支持 Sharp 表达式再编辑，意思时将一行按照给定的 JSON 格式进行转换。可以一对一转换，也可以一对多转换。因为是一个遍历操作，大数据量下不建议使用。占位符格式同 [PUT 语句](/pql/put.md)。

```sql
GET # SELECT varchar1, varchar2, int1 FROM table1;
TRANS # [{ "item_name": "#varchar1", "amount": #int1 }, { "item_name": &varchar2, "amount": #int2 } ];
PUT # INSERT INTO table (item_name, amount) VALUES (&item, #amount);
```

上例 TRANS 语句的作用是将一行数据转成两行，指定新的字段`item_name`和`amount`，将源表`varchar`和`varchar2`的字段的值分别指定给新字段`item_name`，将源表`int`的值指定给新字段`amount`，在 SQL 中，这样一转二的语句着实非常不好写，我是还没学会。

---
参考链接

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