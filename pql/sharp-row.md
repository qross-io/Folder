# Sharp表达式 - 数据行操作
数据行类似于Json中的对象或其他语句中的Map，操作数据行的Link比较少。

* **`GET 'column'`** 获取数据行中名为`column`字段的值。
  ```sql
	VAR $row := SELECT name, age FROM table1 WHERE id=1 -> FIRST ROW;
	SET $name := $row GET 'name';
  ```
    另一种得到行字段值的方法是“点属性法”，详见[集合类型的元素访问](/pql/collection.md)。比如
  ```sql
    SET $name := $row.name;
  ```
  点属性法的执行优先级高于`GET`，常用于遍历数据表时。
  ```sql
    FOR $row OF (SELECT name, age FROM table1) LOOP
        PRINT $row.name;
    END LOOP;
  ```
  当数据行是一个常量时，必须用`GET`。
  ```sql
  OUTPUT # { "name": "Tom", "age": 18，"score": 89 } GET 'name';
  ```
* **`HAS 'column'`** 判断数据行中是否包含名称为`column`的列。
  ```sql
    IF $row HAS 'name' THEN
        PRINT $row.name;
    END IF;
  ```
  非运算为 `IF NOT $row HAS 'name' THEN ...`。也可以使用`UNDEFINED`关键字进行判断。
  ```sql
    IF $row.name IS NOT UNDEFINED THEN
        PRINT $row.name;
    END IF;
  ```
  非运算为 `IF $row.name IS NOT UNDEFINED THEN ...`
* **`REMOVE column1, column2, column3`** 移除数据行中的一个或多字段。
  ```sql
  VAR $row := SELECT name, age FROM table1 WHERE id=1 -> FIRST ROW;
  LET $row REMOVE name, age;
  ```
  上例`$row`的结果是一个空数据行。
* **`SET column1=value1, column2=value2`** 设置数据行中字段的值，也可用于新增加字段。
  ```sql
  LET $row SET name='Tom', age=18, score=89;
  ```
* **`SIZE`** 返回数据行的字段数。上例中`$row SIZE`结果是`3`。
* **`TO TABLE (column1, column2)`** 行转列操作，将数据行转成一个两列的数据表，`column1`和`column2`是新数据表的列名。
  ```sql
    VAR $table := $row TO TABLE (key, value);
    PRINT $table;
  ```
  打印结果为：
  ```json
  [
      {
          "key": "name",
          "value": "Tom"
      },
      {
          "key": "age",
          "value": 18
      },
      {
          "key": "score",
          "value": 89
      }
  ]
  ```

---
参考链接

* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)
* [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)