# 执行字符串形式的语句 EXEC 语句

有些情况下，要执行的语句有可能是一个字符串，比如保存在数据表中的 SELECT 语句。EXEC 语句用于执行字符串形式的语句。

```sql
SET $select := 'SELECT * FROM tabe1';
SET $delete := 'DELETE FROM table1 WHERE id=1';
SET $update := SELECT update_sql FROM table1 WHERE id=2;

EXEC 'GET # '+ $select;
EXEC $delete;
EXEC $update + ' -> IF ZERO 1';
```

**EXEC 语句可以 PQL 支持的所有语句和嵌入规则， 但是每条 EXEC 语句只能执行一条字符串语句。**

---
参考链接

* [数据类型](/pql/datatype.md)
* [变量声明 SET](/pql/set.md)
* [PQL 中的 SQL 语句](/pql/sql.md) 
* [用户变量 $](/pql/variable.md)