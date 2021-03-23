# 返回 PQL 的执行结果 OUTPUT 语句

在 PQL 过程开发时，有的场景需要返回计算结果。比如后端接口开发，基本每个接口都要返回数据给前端。PQL 中使用 OUTPUT 语句或 [RETURN 语句](/pql/return.md)显式输出计算结果。

```sql
OUTPUT # SELECT * FROM table1;  -- 输出数据表
OUTPUT # INSERT INTO …; -- 输出影响的行数
OUTPUT 'Hello World';
OUTPUT [1, 2, 3, 4, 5];  --输出Json数组
```

可以把整个 PQL 计算过程理解为是一个函数，OUTPUT 语句负责把最终结果返回给调用方。

* OUTPUT 语句可以输出任何值，包括常量、任意类型的变量、表达式。在输出时会自动计算结果。
* OUTPUT 关键词后面的井号`#`是可选的，但建议后面使用语句时加上井号。
* 在接口开发中，输出值会自动转变为 Json 格式。PQL 数组或列表类型对应 Json 数组，PQL 数据行对应 Json 对象，PQL 数据表对应 Json 对象数组。
* PQL 与 Json 结合紧密，OUTPUT 可以输出任何自定义格式的 Json 数据，这点在后端接口开发中非常实用。

```sql
OUTPUT {
	"data": ${{ SELECT * FROM table1 }},
	"users": ${{ SELECT id FROM users -> FIRST COLUMN }}
};
```

上例使用了[嵌入式查询表达式](/pql/query.md)，最后的输出数据格式如下：

```json
{
	"data": [
        { "id": 1, "name": "Tom" },
        { "id": 2, "name": "Jerry" }
    ],
	"users": [1, 2, 3, 4]
};
```

OUTPUT 语句并不会中断 PQL 过程的执行，这点与 [RETURN 语句](/pql/return.md)的意义不同。OUTPUT 语句后面依然可以写其他语句。下例中的 PRINT 语句依然会被执行。

```sql
OUTPUT $a;
PRINT 1;
```

如果在一个 PQL 过程中使用了多个 OUTPUT 语句，最后的结果是一个数组，数组的每一项就是每次 OUTPUT 的结果。

## PQL 的隐式输出

开头提到 OUTPUT 语句是“显式输出”计算结果，PQL 还有一种机制叫“隐式输出”。举个例子，在数据计算过程中，单纯的 SELECT 的语句没有任何意义。

```sql
SELECT * FROM table1;
```

但是在后端接口开发中则不然，SELECT 语句可以用来向前端返回数据。在关系型数据库中，任何 SQL 语句都有返回值，查询语句返回一个二维表格，非查询语句返回数据库受影响的行数。Redis 的每个命令也都有返回值。在开发接口时，经常一个接口只有一条 SQL 语句，这时 PQL 不要求在一条语句时仍使用 OUTPUT 语句，PQL 会把这一条语句的结果自动返回。上例默认将SELECT 的结果返回。

隐式输出的规则如下：

* PQL 会把每条有返回结果的语句的执行结果保存在一个列表中。如果整个 PQL 过程没有使用 OUTPUT 语句，隐式结果列表的最后一个结果将被返回; 如果使用了 OUTPUT 语句，则只返回OUTPUT 的结果，隐式结果列表中的结果都会被忽略。

```sql
SELECT * FROM table1;
UPDATE table1 SET name='Tom' WHERE id=3;
```

上例隐式结果列表有两个值，一个是 SELECT 语句的查询结果，另一个是 UPDATE 语句影响数据表的行数，最后 UPDATE 语句的结果会被返回。

```sql
SELECT * FROM table1;
OUTPUT # DELETE FROM table2 WHERE score<60;
UPDATE table1 SET name='Tom' WHERE id=3;
```

上例中 DELETE 语句的结果被 OUTPUT 语句显示输出，最后会被返回，隐式列表中的两个结果将被忽略。总结一下就是 **显式结果会被全部输出，隐式结果只有在没有显示结果时才输出最后一项**。

在 PQL 中除了 OUTPUT 语句还有另一条语句也用来输出和返回数据，比 OUTPUT 功能简单一些，请参阅[ECHO语句](/pql/echo.md)。

---
参考链接

* [简单输出 ECHO](/pql/echo.md)
* [返回结果并中断执行 RETURN](/pql/return.md)
* [PQL 数据类型](/pql/datatype.md)
* [PQL 中的 SQL 语句](/pql/sql.md)
* [更优雅的数据处理方法 Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md) 