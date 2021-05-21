# Sharp 表达式 - 数据表操作

数据表是一种比较特殊的集合类型，概念上是一个二维表格。数据表与关系型数据库的数据表概念一致，也包含列、字段、行等。数据表的某一行的数据类型是“数据行”，数据表某一列的数据列表是“数组”，数据表某一个单元格的数据类型按照数据表对应列的数据类型确定。注意：下面涉及 Json 对象中的字符串一定要用双引号。

## 获取数据表的行、列及单元格

* **`FIRST ROW`** 返回表格的第一行，表格为空时抛出异常。
* **`FIRST ROW row`** 返回表格的第一行，如果表格为空则返回数据行`row`，`row`用 Json 对象表示。
  ```sql
  SELECT * FROM table1 -> FIRST ROW { "name": "Tom", "age": 18 }
  ```
* **`LAST ROW`** 返回表格的最后一行，表格为空时抛出异常。
* **`LAST ROW row`** 返回表格的最后一行，如果表格为空则返回数据行`row`，`row`用 Json 对象表示，同`FIRST ROW`。	
* **`ROW n`** 得到第 n 行，索引从`1`开始，如果不存在则抛出异常。
* **`FIRST COLUMN`** 得到第一列的所有值，返回数组。如果数据表为空，则抛出异常。
* **`FIRST COLUMN array`** 得到表格的第一列的所有值，返回数组。如果数据表为空则返回数组`array`，`array`用 Json 数组表示。
  ```sql
  SELECT * FROM table1 -> FIRST COLUMN [1, 2];
  ```
* **`LAST COLUMN`** 得到表格的最后一列的所有值，返回数组。如果不存在则抛出异常。
* **`LAST COLUMN array`** 得到表格的最后一列的所有值，返回数组。如果数据表为空则返回数组`array`，`array`用 Json 数组表示，同`FIRST COLUMN`。
* **`COLUMN 'column'`** 根据字段名`column`返回此列的所有值，不存在则抛出异常。
* **`FIRST CELL`** 返回第一行第一列的值，数据类型根据第一列的数据类型确定，经常用于返回单值。数据表为空时抛出异常。
* **`LAST CELL`** 返回最后一行最后一列的值，数据类型根据最后一列的数据类型确定。数据表为空时抛出异常。
* **`FIRST ROW CELL 'column'`** 返回第一行列名为`column`列的值。不存在时则抛出异常。
* **`LAST ROW CELL 'column'`** 返回最后一行列名为`column`列的值。不存在时则抛出异常。
* **`HAS 'column'`** 判断数据表中是否包含名称为`column`的列。
* **`RANDOM n`** 从数据表随机取`n`个数据行，结果类型仍是数据表，`n`大于等于`1`且`n`小于等于数据表的记录数，`n`为`1`时返回`1`行的数据表而不是数据行。这个操作会把数据行的顺序打乱，哪怕设置`n`等于数据表的记录数。一般用于数据表的随机取样。

另一种快速获取数据表中内容的方式使用属性规则和索引规则，如`$table.first.field`，详见[集合类型的元素访问](/pql/collection.md)。
  
## 数据表操作

* **`COLLECT (column1, column2, ...) AS 'newColumn'`** 将指定的多个列合并成一列，要合并的列需要大于等于两个，否则无意义。`newColumn`表示新的列名，注意必须加引号。合并的列是一个对象结构，包含合并前每一列的数据。
* **`DELETE`** 通过条件删除数据，条件支持`AND`和`OR`及使用小括号分组，但不支持函数。与`WHERE`操作效果相反。
  ```sql
  SELECT item, COUNT(0) AS amount FROM table1 -> DELETE 'amount = 0';
  ```
* **`INSERT row`** 在数据表中插入一行。还用上面的例子，
  ```sql
  SELECT * FROM table1 -> INSERT { "name": "Tom", "age": 18 };
  ```
* **`INSERT (column1, column2) VALUES (value1, value2)`** 在数据表中插入一行，用 SQL 中 INSERT 语句的类似的语法。
  ```sql
  SELECT * FROM table1 -> INSERT (name, age) VALUES ('Tom', 18};
  ```
* **`INSERT IF EMPTY row`** 如果数据表为空时才插入一行。例子见`INSERT row`。
* **`INSERT IF EMPTY (column1, column2) VALUES (value1, value2)`** 如果数据表为空时才插入一行。
* **`SELECT column1, column2`** 选择数据表中的一列或多列，生成新的数据表，支持`*`和`AS`。几种用法见下例：
  ```sql
  $table SELECT name -- 选择其中一列
  $table SELECT name, age AS years -- 选择其中两列，并重命名其中一列
  $table SELECT * -- 选择全部列，生成的数据表和原数据相同，无意义
  $table SELECT *, name AS title -- 选择全有的全部列，并将其中一列另存为新列。因为数据表中不能存在重名的列，所以新列必须用`AS`重新命名。
  ```
* **`TO HTML TABLE`** 将数据表转化为HTML表格`<table>`字符串，一般用于 Voyager 模板。
* **`TO NESTED MAP 'column'`** 将数据表转化为一个嵌套的 Map 结构。如此非主流的操作来自于前端工程师的变态需求。数据表可以理解本身是一个数据行的数组，而数据行可以理解为是一个 Map 结构。现在前端工程师说数组不好处理，需要一个 Map，Map 每一项的值就是整个数据行。这个操作需要指定一列，前端工程师要的 Map 结构的 Key 就是这列的值（一般来说这列的每个值都是唯一的），然后数据表中每一行的其他值就构成了这个 Map 每一项的 Value。
  ```sql
  SELECT name, age, score FROM students -> TO NESTED MAP;
  ```
  这个接口的返回值是这样的：
  ```json
  {
      "Tom": { "age": 18， "score": 89},
      "Jerry": { "age": 17， "score": 77},
  }
  ```
* **`TO TREE (primaryKey, parentColumn, startPoint, newColumn)`** 将一个数据表转成一个树形结构，这个功能常用于前端目录树的展示，在树形组件渲染中要求一次性获取所有节点的数据。其中`primaryKey`和`parentColumn`分别指定主键字段和父级关系字段，这两个字段用于确定目录的上下级关系。例如主键`id`和父级关系字段`parent_id`，id 为`2`的目录有 3 个子目录，那么这 3 个子目录的 parent_id 值均为`2`，即 id 为`2`的目录。`startPoint`表示顶层目录的父级 id，一般设置为`0`。`newColumn`设置聚合后字段名，如下例中的`children`，当前目录的所有子节点都在这里。特别注意，找到不父级的目录都会被认为是顶级目录。
  ```sql
  SELECT * FROM projects -> TO TREE (id, parent_project_id, 0, children);
  ```
* **`TURN column1 AND column2 TO ROW`** 或 **`TURN (column1, column2) TO ROW`** 将数据表中的`column1`和`column2`两列数据转成数据行, 列`column1`的值做为数据行的字段名，列`column2`的值作为数据行对应字段的值。
  ```sql
  SELECT name, age FROM students -> TURN (name, age) TO ROW;
  ```
* **`TURN TO ROW`** 将数据表第一列和第二列的数据转成数据行，第一列的值作为数据行的字段名，第二列的值作为数据对应字段的值。
* **`WHERE`** 通过条件筛选数据，条件支持`AND`和`OR`及使用小括号分组，但不支持函数。与`DELETE`操作效果相反。
  ```sql
  SELECT item, COUNT(0) AS amount FROM table1 -> WHERE 'amount > 0';
  ```

## 其他操作

* **`COUNT`** 获取数据表的行数，返回一个整数。
* **`FIELDS`** 获得数据表的所有字段名的数组。
* **`LABELS`** 获得数据表中所有字段名标签的数组。
* **`HEADERS`** 获得数据表中的所有字段和标签对应表，返回数据行。

---
参考链接

* [集合类型的元素访问](/pql/collection.md)
* [更优雅的数据操作方法 Sharp 表达式](/pql/sharp.md)
* [Sharp 表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp 表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp 表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp 表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp 表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp 表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp 表达式操作 - 数据判断](/pql/sharp-if.md)
* [Sharp 表达式操作 - Json 字符串](/pql/sharp-json.md)