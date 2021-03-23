# CSV 文件操作

PQL 支持 CSV 文件的读和写，读使用 OPEN 和 SELECT 语句配合，写直接使用 SAVE 语句即可。

## 读取 CSV 文件中的内容

例如现有 CSV 文件`titanic.csv`，表头和前 10 行数据如下：

```
PassengerId,Survived,Pclass,Name,Sex,Age,SibSp,Parch,Ticket,Fare,Cabin,Embarked
1,0,3,"Braund, Mr. Owen Harris",male,22,1,0,A/5 21171,7.25,,S
2,1,1,"Cumings, Mrs. John Bradley (Florence Briggs Thayer)",female,38,1,0,PC 17599,71.2833,C85,C
3,1,3,"Heikkinen, Miss. Laina",female,26,0,0,STON/O2. 3101282,7.925,,S
4,1,1,"Futrelle, Mrs. Jacques Heath (Lily May Peel)",female,35,1,0,113803,53.1,C123,S
5,0,3,"Allen, Mr. William Henry",male,35,0,0,373450,8.05,,S
6,0,3,"Moran, Mr. James",male,,0,0,330877,8.4583,,Q
7,0,1,"McCarthy, Mr. Timothy J",male,54,0,0,17463,51.8625,E46,S
8,0,3,"Palsson, Master. Gosta Leonard",male,2,3,1,349909,21.075,,S
9,1,3,"Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)",female,27,0,2,347742,11.1333,,S
10,1,2,"Nasser, Mrs. Nicholas (Adele Achem)",female,14,1,0,237736,30.0708,,C
```

可使用下列语句进行读取

```sql
OPEN CSV FILE './titanic.csv' AS TABLE 'titanic' WITH FIRST ROW HEADERS;
GET # SELECT * FROM :titanic LIMIT 10;
```

上例中，使用 OPEN 语句打开 CSV 文件并生成一个虚拟表`titanic`，然后可以通过 SELECT 语句进行读取，表名前需要加`:`号。注意这里的 SELECT 语句功能有限，支持有限 WHERE，支持 LIMIT 和分页，但不支持 ORDER BY，更不支持各种函数，而且查询性能也不高。所以不能把 CSV 当做数据库来使用，作者会在将来的某个大版本中对文件查询进行全面优化。

在没有表头的情况下，可以手工定义字段名和数据类型。

```sql
OPEN  CSV FILE './titanic.csv'  AS TABLE 'titanic' (passenger_id INT, Survived BOOLEAN, pclass INT, name TEXT) SKIP 2;
```

上例中`SKIP 2`表示略过前两行，没有`SKIP`表示从第 1 行开始读。

## 将数据另存为 CSV 文件

PQL 不仅提供了输出 [Excel 文件](/pql/excel.md)的方法，还可以输出为 CSV 及其他格式的文本文件。

```sql
OPEN mysql.school;
    GET # SELECT id, name, score FROM students;
SAVE TO NEW CSV FILE "scores.csv"  WITHOUT HEADERS;

```
* 仍使用 GET 语句获取数据。
* `SAVE TO NEW`或`SAVE AS`表示保存为新文件，如果指定的文件存在则先删除。`SAVE TO`表示向现有文件追加数据，如果文件不存在则新建。
* `"scores.csv"`表示保存的文件名，不指定路径默认存放在临时目录(`%QROSS_HOME/temp/`)下，也可以指定完整路径，如 `"/usr/data/temp/scores.csv"`。
* `WITHOUT HEADERS`表示不输出字段名（表头）。
* 如果想输出字段名，可使用`WITH HEADERS`。数据表中的表头经常是英文的，还可以使用`WITH HEADERS`自定义表头。
  ```sql
  SAVE AS "scores.csv" WITH HEADERS (id AS 编号, name AS 姓名, score AS 分数);
  ```

**输出 CSV 文件不需要再写 PUT 语句。**

前端工程师又来了，他说产品经理现在不需要下载 Excel 文件了，现在要下载 CSV 文件。

```sql
SAVE AS CSV STEAM FILE "scores.csv" WITH HEADERS;
```

另存为流文件应用于后端接口开发，需要 Spring Boot 项目和 [OneApi](/oneapi/overview.md) 支持。

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