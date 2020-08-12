# 将数据另存为文本文件
在各个文本文件中，CSV文件只是一个特例。PQL支持自定义文件每行的数据分隔符。
```
OPEN mysql.school;
    GET # SELECT id, name, score FROM students;
SAVE AS TXT FILE "scores.txt" DELIMITED BY ";" WITHOUT HEADERS;
```
* 仍使用GET语句获取数据。
* `SAVE TO NEW`或`SAVE AS`表示保存为新文件，如果指定的文件存在则先删除。`SAVE TO`表示向已有文件追加数据，文件不存在时才新建文件。
* `"scores.txt"`表示保存的文件名，不指定路径默认存放在临时目录(`%QROSS_HOME/temp/`)下，也可以指定完整路径，如 `"/usr/data/temp/scores.txt"`。文本文件的扩展名可任意，如`scores.log`。
* 用`DELIMITED BY`设置每行字段值之间的分隔符，默认是逗号`“,”`。
* `WITHOUT HEADERS`表示不输出字段名（表头）。
* 如果要输出字段名，可使用`WITH HEADERS`。数据表中的表头经常是英文的，还可以使用`WITH HEADERS`自定义表头。
```
SAVE TO NEW TXT FILE "scores.txt" WITH HEADERS (id AS 编号, name AS 姓名, score AS 分数);
```

为了预防前端工程师的特别需求，可以将文件文件输出到流。
```
SAVE AS STEAM FILE "scores.txt" WITH HEADERS;
```
另存为流文件需要Spring Boot项目和[OneApi](/doc/oneapi/overview)支持。


---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [将数据另存为CSV文件](/doc/pql/csv)
* [统一接口输出 OneApi](/doc/oneapi/overview)
