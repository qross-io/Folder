# Sharp 表达式 - Json 字符串操作

虽然在 PQL 中很多地方可以直接写 Json 格式的参数和数据，但有时也需要对 Json 字符串进行处理。Sharp 表达式中提供了处理 Json 字符串的方法，原理和 [PARSE 语句](/pql/parse.md)类似。

* **`FIND path AS TABLE`** 将 Json 字符串中路径 path 下的数据解析成一个数据表。
* **`FIND path AS ROW`** 将 Json 字符串中路径 path 下的数据解析成一个数据行。
* **`FIND path AS ARRAY`** 将 Json 字符串中路径 path 下的数据解析成一个数组。
* **`FIND path AS VALUE`** 将 Json 字符串中路径 path 下的数据解析成一个数值。

其中`path`需传递标准的JsonPath，其中`/`代表根目录。下面看例子：

```sql
'[{
    "name": "Tom",
    "age": 18
},
{
    "name": "Jerry",
    "age": 17
}]' FIND '/' AS TABLE  -- 结果是一个数据表

'{
    "name": "Tom",
    "age": 18
}' FIND '/' AS ROW -- 结果是一个数据行

'[{
    "name": "Tom",
    "age": 18
},
{
    "name": "Jerry",
    "age": 17
}]' FIND '/0' AS ROW  -- 结果是数据行 { "name": "Tom", "age": 18 }

'{
    "name": "Tom",
    "age": 18
}' FIND '/age' AS VALUE -- 结果是数值`18`

'[{
    "name": "Tom",
    "age": 18
},
{
    "name": "Jerry",
    "age": 17
}]' FIND '/1/name' AS VALUE  -- 结果是数值`Jerry`

'{
    "grade": "ClassA",
    "scores": [78, 89, 93, 65]
}' FIND '/scores' AS ARRAY -- 结果是数组`[78, 89, 93, 65]`
```

在 PQL 中建议用数据表、数据行或数组中的操作处理 Json 数据，除非 Json 必须是字符串类型。看下面的例子，已经去掉了 Json 字符串两端的单引号：

```sql
{
    "name": "Tom",
    "age": 18
} GET 'age' -- 结果是数值`18`

[{
    "name": "Tom",
    "age": 18
},
{
    "name": "Jerry",
    "age": 17
}] FIRST ROW -- 结果是数据行 { "name": "Tom", "age": 18 }

[{
    "name": "Tom",
    "age": 18
},
{
    "name": "Jerry",
    "age": 17
}] LAST ROW CELL 'name' -- 结果是数值`Jerry`
```

如果 Json 字符串在变量中，比如从数据库中取出的 Json 数据：

```sql
SET $json := '{ "name": "Tom", "age": 18 }';
OUTPUT # $json! GET 'name';  -- 结果是数据`Tom`
```

对，加个叹号`!`就OK了，叹号的作用是在数据中恢复变量数值时忽略引号。

---
参考链接

* [PQL 中的变量](/pql/variable.md) 
* [解析接口返回的数据 PARSE](/pql/parse.md)
* [更优雅的数据操作方法 Sharp 表达式](/pql/sharp.md)
* [Sharp表达式操作 - 文本和字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 正则表达式 REGEX](/pql/sharp-regex.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)