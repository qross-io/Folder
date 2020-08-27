# PQL中的Json

Json数据格式是当下最流行的数据传输格式，在PQL中每多地方，都可以无障碍使用Json数据，而且Json数据还是PQL中集合类型的表现形式。

## Json与集合类型

### Json数组
在PQL中，Json数组是PQL数组（列表）的表现形式。
```sql
VAR $list := [1, 2, 3, 4, 5];
VAR $str := ["Hello", "World"];
vAR $empty := [];
```
上例声明的变量都是数组，其中`$list`为整数数组，`$str`为字符串数组，`$empty`为空数组。

### Json对象
Json对象是PQL数据行的表现形式。
```sql
VAR $row := { "name": "Tom", "age": 18, "score": 89 };
VAR $empty := {};
```
上例中声明了两个数据行变量，其中`$empty`是一个空数据行。

### Json对象数组
如果Json数组中的元素是对象，那么这个数组就形成了一个数据表。
```sql
VAR $table := [
        { "name": "Tom", "age": 18, "score": 89 },
        { "name": "Jerry", "age": 17, "score": 77 }
    ];
```
上例中声明了一个两行三列的一个数据表，数据表和数据行是PQL中最常用的复合结构。

## 语句中的Json

Json数据可以以常量的形式存在于PQL语句中，作为语句的一部分，用以实现不同的功能。如上面几个用[VAR语句](/pql/var.md)声明的例子。
```sql
SET $name, $age := ["Tom", 18];
IF $a IN [1, 2, 3] THEN ...  -- 也可以 IF $a IN (1, 2, 3) THEN...
FOR $i IN [4, 5, 6] THEN ...
OUTPUT # {
    "name": "Tom", "age": 18, "score": 89
};

REQUEST JSON API 'http://domain.com/api/score' METHOD 'POST'
    SEND DATA { "name": "Tom", "age": 18, "score": 89 };

```

还有很多地方可以使用Json常量数据，相应语句的文档中会介绍。

## Json中嵌入PQL变量和表达式

Json作为一种复合数组结构，可以像[富字符串](/pql/rich.md)一样嵌入各种变量和表达式，以简化计算。
```sql
SET $name := "Tom";
OUTPUT # {
    "students": ${{ SELECT * FROM students }},
    "totla": @COUNT_OF_LAST_SELECT,
    "monitor": $name,
    "date": ${ @NOW FORMAT 'yyyy-MM-dd' }
};
```

上例中嵌入了[用户变量](/pql/variable.md)、[全局变量](/pql/global.md)、[Sharp表达式](/pql/sharp.md)和[查询表达式](/pql/query.md)。如果变量或表达式的计算结果为字符串或日期时间，会自动加 **双引号`"`**。


## Json数据的编辑

PQL中提供了一系列的方法对Json数据进行再编辑。
```sql
SET $list := [1, 2, 3];
LET $list ADD [4, 5, 6];
PRINT $list; -- 结果为 [1, 2, 3, 4, 5, 6];
```

各类型的数据再编辑请分别参见：[数组编辑](/pql/sharp-array)、[数据行编辑](/pql/sharp-row)、[数据表编辑](/pql/sharp-table)和[Json字符串编辑](/pql/sharp-json)。

---
参考链接

* [PQL快速入门](/pql/overview.md)
* [PQL中的数据类型](/pql/datatype.md)
* [变量声明 VAR](/pql/var.md)
* [Sharp表达式](/pql/sharp.md)
* [返回PQL的执行结果 OUTPUT](/pql/output.md)