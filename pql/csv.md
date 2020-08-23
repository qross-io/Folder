# 将数据另存为CSV文件
PQL不仅提供了输出[Excel文件](/pql/excel.md)的方法，还可以输出为CSV及其他格式的文本文件。
```sql
OPEN mysql.school;
    GET # SELECT id, name, score FROM students;
SAVE TO NEW CSV FILE "scores.csv"  WITHOUT HEADERS;
```
* 仍使用GET语句获取数据。
* `SAVE TO NEW`或`SAVE AS`表示保存为新文件，如果指定的文件存在则先删除。`SAVE TO`表示向现有文件追加数据，如果文件不存在则新建。
* `"scores.csv"`表示保存的文件名，不指定路径默认存放在临时目录(`%QROSS_HOME/temp/`)下，也可以指定完整路径，如 `"/usr/data/temp/scores.csv"`。
* `WITHOUT HEADERS`表示不输出字段名（表头）。
* 如果想输出字段名，可使用`WITH HEADERS`。数据表中的表头经常是英文的，还可以使用`WITH HEADERS`自定义表头。
  ```sql
  SAVE AS "scores.csv" WITH HEADERS (id AS 编号, name AS 姓名, score AS 分数);
  ```

**输出CSV文件不需要再写PUT语句。**

前端工程师又来了，他说产品经理现在不需要下载Excel文件了，现在要下载CSV文件。
```sql
SAVE AS CSV STEAM FILE "scores.csv" WITH HEADERS;
```
另存为流文件应用于后端接口开发，需要Spring Boot项目和[OneApi](/oneapi/overview.md)支持。

---
参考链接
* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [将数据保存到Excel文件](/pql/excel.md)
* [将数据另存为Json数据文件](/pql/json-file.md)
* [将数据另存为文本文件](/pql/txt.md)
* [统一接口输出 OneApi](/oneapi/overview.md)