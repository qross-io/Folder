# 解析Json接口返回的数据 - PARSE语句
PARSE语句负责解析接口返回的数据，逻辑和SELECT语句属于同一类，都是从某一个地方查询数据。可以理解REQUEST语句生成了一个Mini数据库，PARSE语句负责从这个Mini数据查询数据。
```
PARSE "path" AS TABLE;
PARSE "path" AS ROW;
PARSE "path" AS ARRAY;
PARSE "path" AS VALUE;
```
* `"path"`参数使用标准的JsonPath表达式。
* `AS`后面表示期望将指定路径的数据解析成什么格式。
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
上例是一个典型的接口返回值的格式。下面看一下用PARSE语句是如何解析的：
```
PARSE '/data' AS TABLE; -- 返回"data"中的数据，解析成一个数据表
PARSE '/message' AS VALUE; -- 返回"success"
PARSE '/data/0' AS ROW; -- 返回数据行 { "name": "Tom", age"": 18 }
PARSE '/data/1/age' AS VALUE; -- 返回数据 17
```

PARSE语句和[SELECT语句](/doc/pql/sql)一样的意义和用法，所以PARSE语句也可以放在[GET语句](/doc/pql/get)里，也可以放在[SET语句](/doc/pql/set)或[VAR语句](/doc/pql/var)后面赋值，或者在[FOR语句](/doc/pql/for)中遍历。可以嵌入变量和表达式，可以用[Sharp表达式](/doc/pql/sharp)对结果数据再编辑，也可以用做[嵌入式查询表达式](/doc/pql/query)。
```
GET # PARSE $path AS TABLE -> INSERT IF EMPTY { "name": "Ted", "age": 19 };
VAR $result := PARSE $path AS $date_type!;
FOR $name IN (PARSE "path" AS ARRAY) LOOP
    PRINT $name;
END LOOP;
```

[REQUEST语句](/doc/pql/request)将请求接口返回的数据保存在缓冲区，只要不再次请求接口，缓冲区的数据就不会被清除，所以可以一次请求多次使用PARSE语句解析。

---
参考链接
* [请求Json数据接口 REQUEST](/doc/pql/request)
* [更优雅的数据操作方法 Sharp表达式](/doc/pql/sharp)
* [Sharp表达式 - Json字符串处理](/doc/pql/sharp-json)
* [数据类型](/doc/pql/datatype)
* [变量声明 SET](/doc/pql/set)
* [变量声明 VAR](/doc/pql/var)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [PQL中的SQL语句](/doc/pql/sql) 
* [循环遍历 FOR](/doc/pql/for)