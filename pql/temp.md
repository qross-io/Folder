# 中间临时数据库 TEMP
Cache数据库是PQL提供的第一个中间数据库，是内存型的，响应及其快速。而TEMP语句构建的中间数据库是PQL提供的第二个中间数据库，是文件型的，响应比Cache数据库稍慢，但在百万级的数据量下，响应仍是非常快速。这里称为 **Temp数据库**。

Temp数据库拥有和Cache数据库几乎一样的功能和使用方法，作者给的建议是小数据量下用Cache数据库，内存放不下的大数据量下用Temp数据库。
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
这里使用和CACHE语句中一样的例子，把`CACHE`关键字改成`TEMP`即可，实现的功能完全相同。TEMP语句拥有CACHE语句相同的规则和注意事项，需要额外说明一个问题：Cache数据库和Temp数据库是两个数据库，两个数据库之间的表不能关联查询（PQL未来可能会提供关联查询的方法）。使用`OPEN TEMP;`语句后，如果要访问其他数据库，仍需要再次使用OPEN语句。

除了拥有Cahce数据库相同的功能外，有一点不一样的是Temp数据库可以通过SAVE AS语句创建多个索引。示例如下：
```sql
    OPEN mysql.school;
        GET # SELECT name, age, score FROM students;
    SAVE AS TEMP TABLE 'students' PRIMARY KEY 'id' UNIQUE KEY (name, age) KEY (score);
```
上例中通过`SAVE AS TEMP TABLE`语句在Temp数据库创建了`students`表并创建了名为`id`的递增主键索引字段，还有`name`和`age`两个字段的联合唯一索引，以及`score`字段的非唯一索引。主键索引只能某一个字段，但是唯一索引和非唯一索引可以由多个字段联合组成。

---
参考链接
* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [中间临时内存数据库 CACHE](/pql/cache.md)