# 中间临时数据库 TEMP

[Cache 数据库](/pql/cache.md)是 PQL 提供的第一个中间数据库，是内存型的，响应及其快速。而 TEMP 语句构建的中间数据库是 PQL 提供的第二个中间数据库，是文件型的，响应比Cache 数据库稍慢，但在百万级的数据量下，响应仍是非常快速。这里称为 **Temp 数据库**，也由 [SQLite 数据库](/note/sqlite/brief.md)提供支持。

Temp 数据库拥有和 Cache 数据库几乎一样的功能和使用方法，作者给的建议是小数据量下用 Cache 数据库，内存放不下的大数据量下用 Temp 数据库。

```sql
OPEN mysql.school;
    TEMP 'students' # SELECT name, age FROM students;
OPEN mysql.classes;
    TEMP 'scores' # SELECT name, score FROM scores;
OPEN TEMP;
    GET # SELECT A.name, A.age, B.score FROM students A INNER JOIN scores B ON A.name=B.name;
SAVE AS mysql.school;
    PUT # INSERT INTO examination (name, age, score);
```

这里使用和 [CACHE 语句](/pql/cache.md)中一样的例子，把`CACHE`关键字改成`TEMP`即可，实现的功能完全相同。TEMP 语句拥有 CACHE 语句相同的规则和注意事项，需要额外说明一个问题：Cache 数据库和 Temp 数据库是两个数据库，两个数据库之间的表不能关联查询（PQL 未来可能会提供关联查询的方法）。使用`OPEN TEMP;`语句后，如果要访问其他数据库，仍需要再次使用 OPEN 语句。

除了拥有 Cahce 数据库相同的功能外，有一点不一样的是 Temp 数据库可以通过 [SAVE 语句](/pql/save.md)创建多个索引。示例如下：

```sql
    OPEN mysql.school;
        GET # SELECT name, age, score FROM students;
    SAVE AS TEMP TABLE 'students' PRIMARY KEY 'id' UNIQUE KEY (name, age) KEY (score);
```

上例中通过`SAVE AS TEMP TABLE`语句在 Temp 数据库创建了`students`表并创建了名为`id`的递增主键索引字段，还有`name`和`age`两个字段的联合唯一索引，以及`score`字段的非唯一索引。主键索引只能某一个字段，但是唯一索引和非唯一索引可以由多个字段联合组成。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [中间临时内存数据库 CACHE](/pql/cache.md)