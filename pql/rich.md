# 富字符串
为了串接方便，PQL和有的语言（比如Javascript、Scala）一样提供了富字符串以接受变量或表达式嵌入传值。普通的字符串可以使用单引号和双引号包含，富字符串使用3个单引号或3个双引号包围。
```sql
SET $id := 1;
SET $rich := '''id IN (SELECT name FROM table1 WHERE id=$id''';
SELECT * FROM users WHERE name $rich!;
```

富字符串不仅方便串接，而且还包含各种功能。

* 富字符串可以像普通字符串一样嵌入到其他语句中，解释器会计算并恢复成正常的字符串结果。
* 富字符串中不支持反斜杠转义，在普通字符串需要输入两次的反斜杠也只需要输入一次即可。如'''c:\io.Qross\Home'''。富字符串中再出现相同类型的单引号或双引号无需再进行转义。
* 富字符中可以嵌入任何PQL支持的占位符类型，如变量、函数、Sharp表达式和富字符串的查询表达式。
* 富字符串可以回行，即可以写成多行。
* 在恢复数据时，富字符串根据自己的引号类型来确定使用哪个引号。一般Json中使用双引号，SQL中使用单引号。
* 富字符串中嵌入的字符串变量或其他恢复成字符串的表达式恢复时不带引号。
* 富字符串中不能再嵌套富字符串。

```sql
SET $hello := 'hello';
SET $name := 'tom';
OUTPUT {
    "title": """$(hello), ${ $name CAPITALIZE }."""
};
```
结果为
```json
{
    "title": "hello, Tom."
}
```

```sql
UPDATE table1 SET description='''Update time is @now''' WHERE id=1;
```
会恢复成
```sql
UPDATE table1 SET description='Update time is 2020-08-08 17:02:33' WHERE id=1;
```

---
参考链接

* [PQL数据类型](/pql/datatype.md)
* [PQL中的变量](/pql/variable.md)
* [数据输出 OUTPUT语句](/pql/output.md)
* [PQL中的SQL语句](/pql/sql.md)
* [更优雅的数据处理方法 Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md) 