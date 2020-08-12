# 执行字符串形式的语句 EXEC语句
有些情况下，要执行的语句有可能是一个字符串，比如保存在数据表中的SELECT语句。EXEC语句用于执行字符串形式的语句。
```
SET $select := 'SELECT * FROM tabe1';
SET $delete := 'DELETE FROM table1 WHERE id=1';
SET $update := SELECT update_sql FROM table1 WHERE id=2;

EXEC 'GET # '+ $select;
EXEC $delete;
EXEC $update + ' -> IF ZERO 1';
```

**EXEC语句可以PQL支持的所有语句和嵌入规则， 但是每条EXEC语句只能执行一条字符串语句。**

---
参考链接
* [数据类型](/doc/pql/datatype)
* [变量声明 SET](/doc/pql/set)
* [PQL中的SQL语句](/doc/pql/sql) 
* [用户变量 $](/doc/pql/variable)