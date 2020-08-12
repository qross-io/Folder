# Sharp表达式 - 数组操作
PQL中的数组是变长的，可以增减元素，按索引访问数据的项。

#### 数组基本操作
设有数组
```
    SET $array := [1, 2, 3, 4, 5];
```
* **`ADD value`** 为数组添加项，支持添加多个值。新增加的值在数组的后面。
  ```
    LET $array ADD 6; -- 添加单个值
    LET $array ADD 7, 8; -- 添加多个值
    LET $array ADD [9, 10, 11]; -- 合并另一个数组的值
  ```
* **`GET index`** 根据索引获取数组的某一个元素，索引从1开始。`$array GET 2`结果是`2`
* **`REMOVE index`** 根据索引删除数组的某一个元素，索引从1开始。`$array REMOVE 2`结果是`[1, 3, 4, 5]`
* **`HEAD`** 获取数组的第一个元素。`$array HEAD`结果是`1`
* **`LAST`** 获取数组的最后一个元素。`$array LAST`结果是`5`
* **`LENGTH`** 获取数组的长度。`$array LENGTH`结果是`5`
* **`REVERSE`** 反转整个数组。`$array REVERSE`结果是`[5, 4, 3, 2, 1]`
* **`RANDOM`** 从数据中随机取一个元素。
* **`RANDOM n`** 从一个集合中随机取`n`个元素，生成一个新的数组，`n`大于`1`且`n`可以大于原数组长度。新数组的元素顺序会被打乱。
* **`JOIN 'delimiter'`** 将数组的所有元素合并成一个字符串，元素之间添加分隔符`delimiter`。`$array JOIN '-'`结果是`'1-2-3-4-5'`。
* **`DELIMIT 'delimiter'`** 将数组的每一项使用分隔符`delimiter`分开并将数组转成一个数据行。`["name=Tom", "age=18", "score=89"] DELIMIT '='`结果是`{ "name": "Tom", "age": "18", "score": "89" }`

#### 其他与数组相关的操作
* **`m TO n`** 生成一个从`m`到`n`的整数数组，包括`n`，一般用于FOR循环中。`1 TO 4`结果是`[1, 2, 3, 4]`
* **`m UNITIL n`** 生成一个从`m`到`n`的整数数组，不包括`n`，一般用于循环中。`1 TO 4`结果是`[1, 2, 3]`。更多示例可以参阅[FOR语句](/doc/pql/for)。
* **`value IN (v1, v2, v3)`** 判断一个值是否在数组中。
* **`value NOT IN (v1, v2, v3)`**  判断一个值是否不在数组中。
  以下是`IN`和`NOT IN`的示例:
  ```
  IF $id IN (SELECT id FROM table1) THEN ...
  IF $id NOT IN (1, 3, 5, 8) THEN ...
  IF $n IN $array THEN ...
  IF $name NOT IN ["Tom", "Jerry", "Garfield"] THEN ...
  ```

---
参考链接
* [更优雅的数据操作方法 Sharp表达式](/doc/pql/sharp)
* [Sharp表达式操作 - 文本和字符串 TEXT](/doc/pql/sharp-text)
* [Sharp表达式操作 - 日期时间 DATETIME](/doc/pql/sharp-datetime)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/doc/pql/sharp-numeric)
* [Sharp表达式操作 - 正则表达式 REGEX](/doc/pql/sharp-regex)
* [Sharp表达式操作 - 数据表 TABLE](/doc/pql/sharp-table)
* [Sharp表达式操作 - 数据行 ROW](/doc/pql/sharp-row)
* [Sharp表达式操作 - 数据判断](/doc/pql/sharp-if)
* [Sharp表达式操作 - Json字符串](/doc/pql/sharp-json)
* [循环遍历 FOR](/doc/pql/for)