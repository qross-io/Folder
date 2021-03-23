# 将数据另存为 Json 数据文件

PQL 还可以将缓冲区的数据保存为 Json 文件，Json 文件的每行都是一个 Json 对象。一般应用于日志存储或数据备份。

```sql
OPEN mysql.school;
    GET # SELECT id, name, score FROM students;
SAVE AS JSON FILE "scores.json";
```

* 还是使用 GET 语句获取数据。
* `SAVE TO NEW`或`SAVE AS`表示保存为新文件，如果指定的文件存在则先删除。`SAVE TO`表示向已有文件追加数据，文件不存在时才新建文件。
* `"scores.json"`表示保存的文件名，不指定路径默认存放在临时目录(`%QROSS_HOME/temp/`)下，也可以指定完整路径，如 `"/usr/data/temp/scores.json"`。

还可以保存为流文件以用于下载需求。

```sql
SAVE AS JSON STEAM FILE "scores.json";
```

另存为流文件需要 Spring Boot 项目和 [OneApi](/oneapi/overview.md)支持。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [将数据另存为 CSV 文件](/pql/csv.md)
* [将数据另存为文本文件](/pql/txt.md)
* [统一接口输出 OneApi](/oneapi/overview.md)