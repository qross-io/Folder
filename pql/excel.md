# 将数据保存到Excel文件
在数据开发中，经常有需求需要将数据导出为Excel。PQL提供了一种极其简单的方式将数据输出成Excel文件。
```
OPEN mysql.classes;
    GET # SELECT id, name, score FROM students;
SAVE AS NEW EXCEL "example.xlsx"  USE TEMPLATE  "/usr/qross/templates/excel/template.xlsx";
    PREP # INSERT INTO sheet1 (A, B, C) VALUES ('编号', '姓名', '分数');
    PUT # INSERT INTO sheet1 ROW 2 (A, B, C) VALUES (id, &name, #score);
```
上例的执行逻辑为：
* 使用[GET语句](/doc/pql/get)获取数据。
* 使用SAVE AS EXCEL语句或SAVE TO EXCEL语句设置要保存的Excel文件。如果使用`SAVE AS`或`NEW`关键字，表示文件如果存在则删除重建。使用`SAVE TO`且不使用`NEW`关键字表示向已有的Excel文件中追加数据，如果不存在则新建。
* 保存的文件（上例`"example.xlsx"`），只写文件名不写路径会保存在临时目录中（`%QROSS_HOME/temp/`），也可以写完整路径，如`"d:\example.xlsx"`。
* 可以基于Excel模板文件生成新的Excel文件，模板中可以预先设定好样式。需要输入模板的完整路径。
* Sheet表单的表头也算数据，可以使用PREP语句执行单条插入语句。
* 列名使用Excel表单中默认列名`A`, `B`, `C`或`1`, `2`, `3`均可。
* 使用PUT语句批量输出数据。
* `ROW`关键字表示从第几行开始插入，不写表示从无数据的行开始插入数据，一般省略。

在接口开发中，前端工程师有时会要求提供下载Excel文件的接口。PQL支持流文件输出，示例
```
SAVE AS EXCEL STREAM FILE "file.xlsx";
    PUT # INSERT INTO sheet1 ROW 2 (A, B, C) VALUES (id, &name, #score);
```
* 文件名位置不需要写路径。
* 流输出方式需要[OneApi](/doc/oneapi/overview)支持。

有时不需要输出Excel文件，CSV文件就可以满足需求，请参阅[另存为CSV文件](/doc/pql/csv)。Excel查询和样式编辑将在未来版本中实现。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [在目标数据源执行非查询操作 PREP](/doc/pql/prep)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [将数据另存为CSV文件](/doc/pql/csv)
* [统一接口输出 OneApi](/doc/oneapi/overview)