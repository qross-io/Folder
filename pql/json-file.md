# 将数据另存为JSON文件
PQL还可以将缓冲区的数据保存为Json文件，Json文件的每行都是一个Json对象。一般应用于日志存储或数据备份。
```
OPEN mysql.school;
    GET # SELECT id, name, score FROM students;
SAVE AS JSON FILE "scores.json";
```
* 还是使用GET语句获取数据。
* `SAVE TO NEW`或`SAVE AS`表示保存为新文件，如果指定的文件存在则先删除。`SAVE TO`表示向已有文件追加数据，文件不存在时才新建文件。
* `"scores.json"`表示保存的文件名，不指定路径默认存放在临时目录(`%QROSS_HOME/temp/`)下，也可以指定完整路径，如 `"/usr/data/temp/scores.json"`。

前端工程师倒是没有要过这种格式的下载需求。
```
SAVE AS JSON STEAM FILE "scores.json";
```
另存为流文件需要Spring Boot项目和[OneApi](/doc/oneapi/overview)支持。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [将数据另存为CSV文件](/doc/pql/csv)
* [将数据另存为文本文件](/doc/pql/txt)
* [统一接口输出 OneApi](/doc/oneapi/overview)
