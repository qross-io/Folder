# 更优雅的数据操作方法 Sharp表达式
Sharp表达式可以对数据进行加工，功能很像SQL中的函数，用于处理各种类型的数据。但Sharp表达式是链式的，不像多个函数是嵌套式的，当对数据进行多步操作时Sharp表达式更清晰易读。Sharp表达式还可以对表达式或语句的结果进行再编辑。正是因为Sharp表达式的存在，才让PQL的功能丰富且强大起来。
```sql
SET $a := 1.23 POW 3 ROUND 2;   -- 取1.23的3次方，保留2位小数
SET $s := "hello world" TAKE AFTER "e" REPLACE FIRST "o" TO "e"; -- 结果是 "lle world"
SELECT * FROM table1 -> INSERT IF EMPTY (id, title) VALUES (1, ‘Test’);
SELECT id FROM table1 -> FIRST COLUMN -> ADD 0;
```
Sharp表达式可以进行多步链式操作，就像函数可以嵌套多层一样。
```sql
    PRINT ${ 'hello world' TAKE AFTER 'hello' TRIM -> TO UPPER };
```
上面示例打印`WORLD`。

如果你已经看过其他语句的文档，应该看到过Sharp表达式的例子。下面是Sharp表达式的基本概念和注意事项：
* SHARP表达式中的每一次操作的名称称为“连接”（Link）。
    + 每一个Link由一个多个英文单词组成，有时会包含数字。
    + Link的左右两端必须有空白字符或单词边界。
    + 如果两个Link之间无参数，建议使用箭头号`->`连接（由减号和大于号构成），否则有可能会出现解析错误。
    + 在语句后面的Link前必须有箭头号`->`，解析程序要区分出哪些是语句，哪些是Sharp表达式。
* 每个Sharp表达式由一个或多个基本式构成，每个基本式由“数据+连接+参数”构成，其中参数有时可省略。每个基本式表示数据的一步加工操作。最后一步基本式的结果及整个Sharp表达式的最后结果。
* 解释器会根据Link名称自动判断并转换要编辑的数据的类型，实在无法处理时，才会抛出异常。
* 有的Link会根据传入的参数多少和类型执行不同的操作，会详细说明。
* 在语句之后Sharp表达式会在语句执行完成之后再进行Link计算。

Sharp表达式为每种数据类型都提供了丰富的处理方法，详情点击下面的分链接。
* [字符串 TEXT](/pql/sharp-text.md)
* [数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [日期时间 DATETIME](/pql/sharp-datetime.md)
* [正则表达式 REGEX](/pql/sharp-regex.md)
* [数组和列表 ARRAY](/pql/sharp-array.md)
* [数据行 ROW](/pql/sharp-row.md)
* [数据表 TABLE](/pql/sharp-table.md)
* [数据判断](/pql/sharp-if.md)

### 将Sharp表达式嵌入到语句中
Sharp表达式的另一个功能是可以像变量一样嵌入到语句中。
```sql
SELECT * FROM table WHERE create_time>${ @NOW MINUS 1 DAY -> FORMAT 'yyyyMMddHHmmss' }!;

-- IF短语句中的例子
UPDATE students SET rate=${ IF $score >= 90 THEN 'A' ELSIF $score >= 75 THEN 'B' ELSIF $score >= 60 THEN 'C' ELSE 'D' END -> CONCAT '+' } WHERE name='Tom';   

-- Sharp表达式与Sharp表达式嵌套
SET $content := $content REPLACE $m TO ${ $m REPLACE "/doc" TO "" CONCAT ".md" };
```
* 嵌入到语句中需要使用`${`和`}`将Sharp表达式包围起来。
* 在`${ }`中可以包含语句，如上例中第二条语句。详细规则见[查询表达式](/pql/query.md)
* 嵌入的Sharp表达式会在语句执行前计算完成并会将计算结果恢复到语句中，根据数据类型自动判断是否需要加引号。如果需要强制取消引号，则在嵌入语句后面加叹号`!`，如上例第一条语句.

上述的Sharp表达式都是作为语句的一部分或者嵌入到其他语句中，不能独立存在。有一种情况例外，在需要对集合变量进行编辑时，只能用Sharp表达式，请参阅 [LET语句](/pql/let.md)。

---
参考链接
* [嵌入式查询表达式](/pql/query.md)
* [集合类型数据编辑 LET](/pql/let.md)
* [Sharp表达式操作 - 字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)