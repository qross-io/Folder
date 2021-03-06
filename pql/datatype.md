# PQL 中的数据类型

PQL 像其他语言一样也有自己的数据类型，不过在 PQL 中，数据类型不需要显式声明。在多数据情况下，使用者并不需要关心变量的数据类型，解释器会自动识别和转化。  
PQL 数据类型分为三类：基本数据类型、复合数据类型和扩展数据类型。基本数据类型包括整数、小数、字符串、日期时间、布尔和正则表达式; 复合数据类型包括数组、数据行、
数据表; 扩展数据类型使用非常少，可以理解为 Java 的类，先不做介绍。

## 整数 INTEGER

整数不区分整形和长整型，所有整数都以长整形存储。

```sql
SET $i := 1;
UPDATE table1 SET name='Tom' WHERE id=$i;
```

## 小数 DECIMAL

同样，小数也不区分单精度数和双精度数，所有小数都以双精度数存储。

```sql
SET $d := 3.1415926;
PRINT ${ $d ROUND 2 }; -- 输出 3.14
```

## 字符串 TEXT

与其他语言的字符串意义相同。

```sql
SET $name := 'Tom';
SELECT score FROM table1 WHERE name=$name;
```

字符串可以使用单引号包围也可以使用双引号包围，如`"hello 'world'"`或`'hello "world"'`，即单引号字符串中的双引号或双引号字符串中的单引号都不需要转义。字符串使用反斜杠`\`作为转义字符。

* 单引号字符串中的单引号或双引号字符串中的双引号都需要转义，如`"hello \"world\""`或`'hello \'world\''`。
* 无论是单引号字符串还是双引号字符串，输出反斜杠`\`都需要转义。如`"c:\\io.Qross\\PQL"`
* 输出回车符`\r`、换行符`\n`、制表符`\t`等特殊字符与其他语言一致。如`"\n"`。

像 Scala 和 Javascript 一样，PQL 提供富字符串提供各种嵌入功能，详见[富文本字符串](/pql/rich.md)。字符串相关操作方法详见 [Sharp表达式](/pql/sharp-text.md)

## 日期时间 DATETIME

PQL 不区分日期类型和时间类型，统一表示为日期时间类型。日期时间类型的文本格式为`yyyy-MM-dd HH:mm:ss`，如`2020-08-01 22:02:37`。

```sql
SET $yesterday := @TODAY MINUS 1 DAY;
SET $hour := @NOW GET HOUR;
SET $day := '2020-08-02';
```

* 如果声明时未指定时间，则各时间单位的值都默认为`0`，如`2020-08-15`等同于`2020-08-15 00:00:00`。
* PQL 可以自动识别很多格式的时间，如`yyyyMMddHHmmss`格式，但是只有在进行操作时才会转换，详见[日期时间相关的 Sharp 表达式](/pql/sharp-datetime.md)。
* PQL 提供了非常多的方法来处理时间和其他的数据类型，如例子中的`MINUS DAYS`和`GET HOUR`，详见 [Sharp 表达式](/pql/sharp-datetime.md)

## 布尔 BOOLEAN

布尔类型主要用于判断条件式的值，`true`和`false`是可选的两个值（不区分大小写）。

```sql
WHILE true LOOP
    ......
    EXIT WHEN $i == 1;
END LOOP;
```

与其他语言不同的是，当做为条件判断时，系统会自动识别要判断的值是否能转化为布尔值。转化规则如下：

* 字符串 `'yes'`, `'true'`, `'on'`, `'ok'` 转换为 `true`，字符串 `'no'`, `'false'`, `'off'` 转换为 `false`。
* 数字大于`0`转化为`true`，小于等于`0`转换为`false`。
* 集合类型，不为空转化为`true`，为空转化为`false`。
  
## 正则表达式 REGEX

正则表达式在 PQL 中使用较少，主要用于字符串匹配。

```sql
SET $r := "\d+";
SET $letter := "(?i)^[a-z]$";
```

格式与字符串完全一致，进行操作时解释器会自动判断类型。语法规则与其他语言一致，请查阅网络上的其他文章，注意不区分大小写请在正则表达式前面加`(?i)`。相关的操作方法请参见 [Sharp 表达式](/pql/sharp-regex.md)。

## 数组 ARRAY

数组也可以称为列表，没有其他语言中的各种严格限制。数组概念和 Json 中的数组一致，也可以理解为是表格中的一列。

```sql
VAR $list := [1, 2, 3, 4, 5];
VAR $scores := SELECT score FROM table1 -> FIRST COLUMN;
```

数组在 PQL 中会经常用到。可以使用 FOR 语句进行遍历。

```sql
FOR $i IN $list LOOP
    PRINT $i;
END LOOP;
```

数组不是定长的，可任意增减元素，数组操作见 [Sharp 表达式](/pql/sharp-array.md)。

## 数据行 ROW

数据行可以理解为 Json 中的对象结构或者 HashMap，但数据行字段的名称只能为字符串，且每个字段均有数据类型，可以保存不同类型的值。

```sql
VAR $row := { "name": "Tom", "score": 89 };
VAR $tom := SELECT name, score FROM table1 -> FIRST ROW;
```

可使用`.`属性规则访问数据行的值。如`$row.name`。数据行中的字段名不区分大小写，建议使用时小写。详见[集合类型的元素访问](/pql/collection.md)

## 数据表 TABLE

PQL 是一种数据处理语言，二维表格是数据处理中最常用的数据结构，PQL 提供了一种功能强大的表格数据结构。简单的可以理解为是数据行的数组，其输出也是一个 Json 的对象数组。

```sql
VAR $students = [{ "name": "Tom", "score": 89 }, { "name": "Jerry", "score": 77 }];
VAR $table := SELECT name, score FROM table1;
```

可以使用 FOR 循环对表格进行遍历。

```sql
FOR $name, $score IN (SELECT name, score FROM table1) LOOP
    PRINT $name;
END IF;
```

或者

```sql
FOR $student OF (SELECT name, score FROM table1) LOOP
    PRINT $student.name;
END IF;
```

数据表中的字段名不区分大小写，数据表相关操作参见 [Sharp 表达式](/pql/sharp-table.md)

## 哈希列表 HASHSET

与数据或列表类似，数组中的值允许重复，但哈希列表中的值不允许重复，且哈希列表的元素是无序的。哈希列表在 PQL 中应用非常少，不能直接声明，只能通过数组转化而来。所有操作跟数组完全相同，不再赘述。

```sql
VAR $hashset := [1, 3, 3, 2, 5, 2, 4, 5, 1] TO HASHET;
VAR $scores := SELECT score FROM table1 -> FIRST COLUMN -> TO HASHSET;
```

## 扩展类型

*扩展数据类型将在未来版本中支持。*

---
参考链接

* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [更优雅的数据操作方法 Shap 表达式](/pql/sharp.md) 
* [Sharp表达式操作-字符串](/pql/sharp-text.md)
* [Sharp表达式操作-整数和小数](/pql/sharp-numeric.md)
* [Sharp表达式操作-日期时间](/pql/sharp-datetime.md)
* [Sharp表达式操作-数组](/pql/sharp-array.md)
* [Sharp表达式操作-数据行](/pql/sharp-row.md)
* [Sharp表达式操作-数据表](/pql/sharp-table.md)
* [集合类型的元素访问](/pql/collection.md)
* [条件循环 WHILE](/pql/while.md)