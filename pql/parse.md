# 解析 Json 接口返回的数据 - PARSE 语句

PARSE 语句负责解析接口返回的数据，逻辑和 SELECT 语句属于同一类，都是从某一个地方查询数据。可以理解 REQUEST 语句生成了一个 Mini 数据库，PARSE 语句负责从这个 Mini 数据查询数据。

```sql
PARSE "path" AS TABLE;
PARSE "path" AS ROW;
PARSE "path" AS OBJECT;
PARSE "path" AS ARRAY;
PARSE "path" AS LIST;
PARSE "path" AS VALUE;
```

`"path"`参数使用标准的 JsonPath 表达式。`AS`后面表示期望将指定路径的数据解析成什么格式。其中`ROW`和`OBJECT`等价、`LIST`和`ARRAY`等价。

```json
{
    "timestamp": 132,
    "message": "success",
    "data": [{
            "name": "Tom",
            "age": 18
        },
        {
            "name": "Jerry",
            "age": 17
        }]
}
```

上例是一个典型的接口返回值的格式。下面看一下用 PARSE 语句是如何解析的：

```sql
PARSE '/data' AS TABLE; -- 返回"data"中的数据，解析成一个数据表
PARSE '/message' AS VALUE; -- 返回"success"
PARSE '/data/0' AS ROW; -- 返回数据行 { "name": "Tom", age"": 18 }
PARSE '/data/1/age' AS VALUE; -- 返回数据 17
```

PARSE 语句和 [SELECT 语句](/pql/sql.md)一样的意义和用法，所以 PARSE 语句也可以放在 [GET 语句](/pql/get.md)里，也可以放在 [SET 语句](/pql/set.md)或 [VAR 语句](/pql/var.md)后面赋值，或者在 [FOR 语句](/pql/for.md)中遍历。可以嵌入变量和表达式，可以用 [Sharp 表达式](/pql/sharp.md)对结果数据再编辑，也可以用做[嵌入式查询表达式](/pql/query.md)。

```sql
GET # PARSE $path AS TABLE -> INSERT IF EMPTY { "name": "Ted", "age": 19 };
VAR $result := PARSE $path AS $date_type!;
FOR $name IN (PARSE "path" AS ARRAY) LOOP
    PRINT $name;
END LOOP;
```

[REQUEST 语句](/pql/request.md)将请求接口返回的数据保存在缓冲区，只要不再次请求接口，缓冲区的数据就不会被清除，所以可以一次请求多次使用 PARSE 语句解析。

---
参考链接

* [请求 Json 数据接口 REQUEST](/pql/request.md)
* [更优雅的数据操作方法 Sharp 表达式](/pql/sharp.md)
* [Sharp 表达式 - Json字符串处理](/pql/sharp-json.md)
* [数据类型](/pql/datatype.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [PQL 中的 SQL 语句](/pql/sql.md) 
* [循环遍历 FOR](/pql/for.md)