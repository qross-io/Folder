# PQL概览
PQL是一种跨数据源的过程化查询语言，是一种运行在JVM上的中间件语言。门槛极低，会SQL即可编写数据处理程序。设计的初忠是尽可能简化数据处理过程。PQL不仅应用于数据开发，还可应用于后端开发和模板引擎等应用场景，可极大简化代码，更简单的处理和呈现呈现数据。清晰的代码格式易于规范开发流程，方便统一管理。  
PQL旨在提供一种最简单的方式对数据处理过程的各种查询语句进行封装，让开发过程更专注于业务逻辑。PQL集成了大量附加功能，所有相关功能都可用一条语句实现，简单高效。  
Qross项目大量使用了PQL，从模板引擎到后端接口、从查询工具到调度工具都在使用PQL。
PQL看起来很像存储过程。每条语句使用分号`;`结尾，更多语法规则见[PQL基本语法](/doc/pql/basic)
1. PQL的Hell World：  
```
    PRINT 'HELLO WORLD';
```
2. PQL支持连接任意JDBC数据源，例如:
```
    OPEN mysql.qross;
    SELECT * FROM tasks LIMIT 10;
```
3. 跨数据源数据流转非常轻松：
```
    OPEN hive.cluster1;
        GET # SELECT name, COUNT(0) AS amount FROM table1 GROUP BY name;
    SAVE TO mysql.result;
        PUT # INSERT INTO table2 (name, amount) VALUES (&name, #amount);
```
4. 提供中间数据库缓存处理过程中的数据：
```
    OPEN mysql.db1;
        CACHE 'table1' # SELECT id, name FROM table1;
    OPEN mysql.db2;
        CACHE 'table2' # SELECT id, score FROM table2;
    OPEN CACHE;
        SELECT A.id, A.name, B.score FROM table1 A INNER JOIN table2 B ON A.id=B.id;       
```
5. 可无障碍使用JSON数据：
```
    OUTPUT {
        "data": ${{ SELECT * FROM table1 }},
        "count": @COUNT_OF_LAST_SELECT
    };
```
6. 支持各种形式的变量和表达式嵌入以对数据进行再加工：
```
SET $name := SELECT name FROM table1 WHERE id=#{id};
SELECT * FROM table2 WHERE name=$name AND create_time>=${ @NOW MINUS 1 DAY }
     -> INSERT IF EMPTY (name, score) VALUES ('N/A', 0);　
```
7. 支持条件表达式IF和CASE：
```
    IF $i > 1 THEN
        PRINT 'greater than 1.';
    ELSIF $i < 1 THEN
        PRINT 'less then 1.';
    ELSE
        PRINT 'equals 0.';
    END IF;
```
8. 支持循环语句FOR和WHILE：
```
    FOR $id, $name IN (SELECT id, name FROM table3) LOOP
        PRINT $id;
        PRINT $name;
        SLEEP 1 SECOND;
    END LOOP;
```
9. 支持请求数据接口和发送邮件：
```
REQUEST JSON API 'http://www.domain.com/api?id=1'
    PARSE '/data' AS TABLE;
```
10. 支持Excel、CSV、TXT等文件的读写操作：
```
OPEN mysql.db1;
    GET # SELECT * FROM table1;
SAVE AS NEW EXCEL "example.xlsx"  USE TEMPLATE  "template.xlsx";
    PREP # INSERT INTO sheet1 (A, B, C) VALUES ('姓名', '年龄', '分数');
    PUT # INSERT INTO sheet1 ROW 2 (A, B, C) VALUES (id, ‘#name’, &title);
``` 
PQL的最大的特点就是“简单”，可以在你的任何Java或Scala项目中使用。更多更强大的功能请参阅各语句对应的文档。

---
参考链接
* [使用PQL](/doc/pql/use-pql)
* [PQL基本语法](/doc/pql/basic)