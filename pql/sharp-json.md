# Sharp表达式 - Json字符串操作
虽然在PQL中很多地方可以直接写Json格式的参数和数据，但有时也需要对Json字符串进行处理。Sharp表达式中提供了处理Json字符串的方法，原理和[PARSE语句](/doc/pql/parse)类似。

* **`FIND path AS TABLE`** 将Json字符串中路径path下的数据解析成一个数据表。
* **`FIND path AS ROW`** 将Json字符串中路径path下的数据解析成一个数据行。
* **`FIND path AS ARRAY`** 将Json字符串中路径path下的数据解析成一个数组。
* **`FIND path AS VALUE`** 将Json字符串中路径path下的数据解析成一个数值。

其中`path`需传递标准的JsonPath，其中`/`代表根目录。下面看例子：
```
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

在PQL中建议用数据表、数据行或数组中的Link处理Json数据，除非Json必须是字符串类型。看下面的例子，已经去掉了Json字符串两端的单引号：

```
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

如果Json字符串在变量中，比如从数据库中取出的Json数据：
```
    SET $json := '{ "name": "Tom", "age": 18 }';
    OUTPUT # $json! GET 'name';  -- 结果是数据`Tom`
```
对，加个叹号`!`就OK了，叹号的作用是在数据中恢复变量数值时忽略引号。

---
参考链接
* [PQL中的变量](/doc/pql/variable) 
* [解析接口返回的数据 PARSE](/doc/pql/parse)
* [更优雅的数据操作方法 Sharp表达式](/doc/pql/sharp)
* [Sharp表达式操作 - 文本和字符串 TEXT](/doc/pql/sharp-text)
* [Sharp表达式操作 - 日期时间 DATETIME](/doc/pql/sharp-datetime)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/doc/pql/sharp-numeric)
* [Sharp表达式操作 - 正则表达式 REGEX](/doc/pql/sharp-regex)
* [Sharp表达式操作 - 数组 ARRAY](/doc/pql/sharp-array)
* [Sharp表达式操作 - 数据行 ROW](/doc/pql/sharp-row)
* [Sharp表达式操作 - 数据表 TABLE](/doc/pql/sharp-table)
* [Sharp表达式操作 - 数据判断](/doc/pql/sharp-if)