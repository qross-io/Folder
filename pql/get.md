# 将数据保存在缓冲区 GET
PQL构建了一个缓冲区用来保存GET语句或PASS语句获取的数据，以方便将这些数据更新到目标数据源或者将数据输出到其他地方。在[SAVE语句](/pql/save.md)中，已经展示了GET语句的基本用法。例如从当前数据源获取数据：
```sql
GET # SELECT * FROM table1;
```
缓冲区的结构其实就是一个二维表格，这个二维表格将每次GET到的数据`UNION ALL`到一起。
```sql
GET # SELECT * FROM table1;
GET # SELEcT * FROM table2;
```
也可以从多个数据源GET数据。
```sql
OPEN mysql.db1;
    GET # SELECT * FROM table1;
OPEN oracle.db2;
    GET # SELEcT * FROM table2;
```
**无论GET多少次，缓冲区中只会有一个数据表。** 强烈建议每次GET数据结构完全一致，如果不一致在PUT数据或输出时有可能产生与预期不一样的结果。  
GET语句不仅支持SELECT语句，理论上支持任何语句，如果语句返回的不是二维表格，将自动转成表格。
```sql
GET # REDIS HGETALL hashkey;
```
以上示例中，Redis的HGETALL命令返回的是一个Hash结构数据，类似于Json中的对象结构，在PQL中是一个数据行，可以理解为是二维表格中的某一行，有多个`键-值`对。PQL自动将这个数据行转行一行的表格，键名即表格中的字段名。  
GET也可以将其他集合数据结构放到缓冲区中。
```sql
GET # [1, 2, 3, 4, 5, 6, 7, 8, 9];
```
GET语句将上面的数据转成一个九行一列的表格，字段名默认为`item`。  
在保存到缓冲区之间可以会数据进行再加工。
```sql
GET # SELECT name, score FROM table1 -> INSERT (name, score) VALUES ('Tom', 89);
GET # REDIS hgetall students -> TO TABLE (name, score);
```
上面示例中，第一个GET在查询结果中增加了一行数据，然后再保存到缓冲区。第二个GET将取到的Hash结构的数据 **行转列** 成两列的表格，列表分别为`name`和`score`。`->`之后的内容称为[Sharp表达式](/pql/sharp.md)，更多相关操作见对应章节。
GET可以将单值也转成表格放进缓冲区，默认列名为`value`，但是这个应用场景非常少。
```sql
GET # "Hello World";
```

在GET完成之后，有几个全局变量可用：
* `@COUNT_OF_LAST_GET` 最后一次GET的数据量（表格行数），可简写为`@COUNT`
* `@TOTAL_COUNT_OF_RECENT_GET` 缓冲区中的数据总量，可简写为`@TOTAL`
* `@BUFFER` 可以通过这个全局变量访问缓冲区
* `@COUNT_OF_LAST_SELECT` 如果GET语句中使用SELECT查询，则这个变量也可以得到GET的数据量，可简写为`@ROWS`
```sql
GET # SELECT name FROM table1;
FOR $row OF @BUFFER LOOP
    PRINT $row.score;
END LOOP;
```

**特别注意**

* GET语句的作用仅是将数据保存在缓冲区，以备后续操作。其本身并不会向PQL过程返回任何数据（如后端接口开发中需要向客户端返回数据），返回数据请使用[OUTPUT语句](/pql/output.md)。
* 在GET时一定要注意当前打开的数据源，特别是在控制结构中。如果不确定，可以再OPEN一次，对于已经OPEN过的数据源，不会有任何性能问题。
* 缓冲区数据的生命周期是在第一次GET开始至PUT一次数据之后的下次GET之前。详细点，GET可以多次，且多次GET之间是`UNION ALL`的关系，如果没遇到PUT或其他数据输出操作，会一直累加下去; 第一次PUT之后，表示GET已经完成了这次工作，可以将这次缓冲的数据清掉了，但这时仍可以执行多次PUT。再次使用GET，即会清空缓冲区的数据并开始新一轮的GET。如果没有后续的GET语句，PQL会在运行完成时清空缓冲区。
* 暂时不支持缓冲区数据的再编辑，这个场景应用非常少，会在未来版本中提供。现在可使用Sharp表达式在保存到缓冲区之前对数据进行编辑，如果是非`UNION ALL`操作，可以将数据临时保存到中间内存数据库，见[CACHE语句](/pql/cache.md)。
* 缓冲区存储的数据量受JVM限制，一般经验是不超过20000行。如遇大量数据流转的场景，请使用GET的多线程版本[PAGE语句](/pql/page.md)和[BLOCK语句](/pql/block.md)代替。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)
* [全局变量 @](/pql/global-variable.md)