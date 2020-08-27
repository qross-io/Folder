# Sharp表达式 - 判断操作
在数据处理过程中，有时会不确定上一步的数据是否返回预期的值，这时会用到数据判断操作。基本逻辑就是：假设没有得到我们预期的值，可以再赋一个默认值。我们这[变量](/pql/variable.md)中介绍过变量的几种状态，这里的判断规则与其相同。

### 判断数据是否为空
* **`IF EMPTY value`**	如果数据为空，则赋值为`value`。可用于判断数据表、数据行、数组和字符串。
* **`IF NOT EMPTY value`** 如果数据不为空，则赋值为`value`，可用于判断数据表、数据行、数组和字符串。

> 数据表为空表示数据表中没有任何数据行。
> 数据行为空表示数据行中没有任何字段。
> 数组为空表示数组中没有任何元素。
> 字符串为空表示字符串中没有任何字符。

```sql
SELECT * FROM table1 -> IF EMPTY [{ "name": "Tom", "age": 18 }]; -- 效果同 INSERT IF EMPTY { "name": "Tom", "age": 18 }
OUTPUT # $row.create_time IS EMPTY 'N/A';
```

### 判断数据是否为`NULL`
* **`IF NULL value`** 如果数据为`NULL`，则赋值为`value`。可用于判断数据为取出的值是否为`NULL`，变量是否赋值，变量值是否有异常等。
* **`IF NOT NULL value`** 如果数据不为`NULL`，则赋值为`value`。
* **`IF UNDEFINED value`** 用于判断变量是否已定义或参数#{name}是否已传值。

```sql
SET $name, $age, $score := ["Tom", 18];
OUTPUT # $name IF NOT NULL 'Jerry';
OUTPUT # $score IF NULL 60;
SET $rate := '#{rate}' IF UNDEFINED 'C'; 
```
一般情况下`IF NULL`和`IF UNDEFINED`可以通用。

### 数据的特殊值判断
下面几个Link使用场景较少。

* **`IF TRUE value`** 如果值为真则赋值为`value`。
* **`IF FALSE value`** 如果值为假则赋值为`value`。
* **`IF ZERO m`** 如果给定的数字为`0`则赋值为`m`。
* **`IF NOT ZERO m`**  如果不为`0`则赋值为`m`。


**以上介绍所有Link中默认值`value`的数据类型可以与要判断的数据的数据的类型不同。**

---
参考链接

* [PQL中的变量](/pql/variable.md) 
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)