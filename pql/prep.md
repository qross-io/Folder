# 目标数据源执行非查询操作 PREP
在数据开发过程中，经常遇到的场景就将计算结果保存结果表中，但是经常碰到计算程序重复执行的情况，如重置数据或执行失败。这时就需要在保存数据之前对目标数据源进行一次更新或删除操作。
```sql
OPEN mysql.db1;
    GET # SELECT name, score FROM table1;
OPEN mysql.db2;
    DELETE FROM table2;
SAVE AS mysql.db2;
    PUT # INSERT INTO table2 (name, score) VALUES (?, ?);
```
上面示例的逻辑是在保存数据之前对数据库`db2`的数据表`table2`进行一次删除操作。逻辑正确，但是有一步从`db1`到`db2`的操作。上面的示例可以简化为：
```sql
OPEN mysql.db1;
    GET # SELECT name, score FROM table1;
SAVE AS mysql.db2;
    PREP # DELETE FROM table2;
    PUT # INSERT INTO table2 (name, score) VALUES (?, ?);
```
PREP表示“预备”的意思，作用是在目标数据源执行一次非查询即更新操作， **只支持非查询语句，如UPDATE、INSERT、DELETE、REPLACE等**。

PREP语句执行之后，会将这次更新影响数据库的行数保存在全局变量 `@AFFECTED_ROWS_OF_LAST_PREP`中，可以在其他逻辑中使用。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [全局变量 @](/pql/global.md)
* [PQL中的SQL语句](/pql/sql.md) 