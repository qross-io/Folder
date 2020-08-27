# Sharp表达式 - 数组操作
PQL中的数组是变长的，可以增减元素，按索引访问数据的项。

### 数组基本操作
设有数组
```sql
    SET $array := [1, 2, 3, 4, 5];
```
* **`ADD value`** 为数组添加项，支持添加多个值。新增加的值在数组的后面。
  ```sql
    LET $array ADD 6; -- 添加单个值
    LET $array ADD 7, 8; -- 添加多个值
    LET $array ADD [9, 10, 11]; -- 合并另一个数组的值
  ```
* **`GET index`** 根据索引获取数组的某一个元素，索引从`1`开始。`$array GET 2`结果是`2`。另一种获取数组中元素的方法是使用方括号`[]`，例如`$array[2]`，注意索引从`0`开始，详见[集合类型的元素访问](/pql/collection.md)。
* **`HAS item`** 判断数组中是否包含元素`item`。`$array HAS 6`结果是`false`
* **`REMOVE index`** 根据索引删除数组的某一个元素，索引从`1`开始，可以同时删除多个值。`$array REMOVE 2, 3`结果是`[1, 4, 5]`
* **`FIRST`** 获取数组的第一个元素。`$array FIRST`结果是`1`。也可以用`$array.first`，优先级高于`FIRST`，详见[集合类型的元素访问](/pql/collection.md)
* **`LAST`** 获取数组的最后一个元素。`$array LAST`结果是`5`。也可以用`$array.last`，优先级高于`LAST`，详见[集合类型的元素访问](/pql/collection.md)
* **`LENGTH`** 获取数组的长度。`$array LENGTH`结果是`5`
* **`REVERSE`** 反转整个数组。`$array REVERSE`结果是`[5, 4, 3, 2, 1]`
* **`SORT`或`ASC`** 对数组元素进行正序排序，只支持数字和字符串元素的排序。`[3, 1, 2] SORT`的结果是`[1, 2, 3]`
* **`DESC` 对数组元素进行倒序排序，只支持数字和字符串元素的排序。`[3, 1, 2] DESC`的结果是`[3, 2, 1]`
* **`RANDOM`** 从数据中随机取一个元素。
* **`RANDOM n`** 从一个集合中随机取`n`个元素，生成一个新的数组，`n`大于`1`且`n`可以大于原数组长度。新数组的元素顺序会被打乱。
* **`JOIN 'delimiter'`** 将数组的所有元素合并成一个字符串，元素之间添加分隔符`delimiter`。`$array JOIN '-'`结果是`'1-2-3-4-5'`。
* **`DELIMIT 'delimiter'`** 将数组的每一项使用分隔符`delimiter`分开并将数组转成一个数据行。`["name=Tom", "age=18", "score=89"] DELIMIT '='`结果是`{ "name": "Tom", "age": "18", "score": "89" }`

### 其他与数组相关的操作
* **`m TO n`** 生成一个从`m`到`n`的整数数组，包括`n`，一般用于FOR循环中。`1 TO 4`结果是`[1, 2, 3, 4]`
* **`m UNITIL n`** 生成一个从`m`到`n`的整数数组，不包括`n`，一般用于循环中。`1 TO 4`结果是`[1, 2, 3]`。更多示例可以参阅[FOR语句](/pql/for.md)。
* **`value IN (v1, v2, v3)`** 判断一个值是否在数组中。
* **`value NOT IN (v1, v2, v3)`**  判断一个值是否不在数组中。
  以下是`IN`和`NOT IN`的示例:
  ```sql
  IF $id IN (SELECT id FROM table1) THEN ...
  IF $id NOT IN (1, 3, 5, 8) THEN ...
  IF $n IN $array THEN ...
  IF $name NOT IN ["Tom", "Jerry", "Garfield"] THEN ...
  ```

---
参考链接

* [集合类型的元素访问](/pql/collection.md)
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)
* [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)
* [循环遍历 FOR](/pql/for.md)