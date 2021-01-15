# PQL概览
PQL是一种跨数据源的过程化查询语言（Procedural Query Language），是一种运行在JVM上的中间件语言。PQL的门槛极低，会SQL即可编写数据处理程序。PQL旨在提供一种最简单的方式对数据处理过程的各种查询语句进行封装，让开发过程更专注于业务逻辑。PQL中集成了大量附加功能，所有相关功能都可用一条语句实现，简单高效。    
PQL不仅可应用于数据开发（特别是多数据源场景下数据流转非常方便），还可应用于后端开发和模板引擎等场景，可极大简化代码，更简单的处理和呈现数据。PQL清晰的代码格式易于规范开发流程，方便统一管理。  

PQL看起来很像存储过程。每条语句使用分号`;`结尾，更多语法规则见[PQL基本语法](/pql/basic.md)

1. PQL的 `Hello World`：

    ```sql
    PRINT 'Hello World!';
    ```
2. PQL支持[连接任意JDBC数据源](/pql/properties.md)并顺序[执行SQL语句](/pql/sql.md)，例如:

    ```sql
    OPEN mysql.qross;
        INSERT INTO scores (name, score) VALUES ('Tom', 89);
        UPDATE students SET age=18 WHERE id=1;
        SELECT * FROM scores ORDER BY score LIMIT 10;
    ```
3. [跨数据源数据流转](/pql/save.md)非常轻松：

    ```sql
    OPEN hive.cluster1;
        GET # SELECT name, COUNT(0) AS amount FROM table1 GROUP BY name;
    SAVE TO mysql.result;
        PUT # INSERT INTO table2 (name, amount) VALUES (&name, #amount);
    ```
4. 提供[中间数据库](/pql/cache.md)缓存处理过程中的数据：

    ```sql
    OPEN mysql.db1;
        CACHE 'table1' # SELECT id, name FROM table1;
    OPEN mysql.db2;
        CACHE 'table2' # SELECT id, score FROM table2;
    OPEN CACHE;
        SELECT A.id, A.name, B.score FROM table1 A INNER JOIN table2 B ON A.id=B.id;       
    ```
5. 可无障碍[使用JSON数据](/pql/json.md)：

    ```sql
    OUTPUT {
        "data": ${{ SELECT * FROM table1 }},
        "count": @COUNT_OF_LAST_SELECT
    };
    ```
6. 支持各种形式的[变量](/pql/variable.md)和[表达式](/pql/sharp.md)嵌入以对数据进行再加工：

    ```sql
    SET $name := SELECT name FROM table1 WHERE id=#{id};
    SELECT * FROM table2 WHERE name=$name AND create_time>=${ @NOW MINUS 1 DAY }
        -> INSERT IF EMPTY (name, score) VALUES ('N/A', 0);　
    ```
7. 支持条件控制语句[IF](/pql/if.md)和[CASE](/pql/case.md)：

    ```sql
    IF $i > 1 THEN
        PRINT 'greater than 1.';
    ELSIF $i < 1 THEN
        PRINT 'less then 1.';
    ELSE
        PRINT 'equals 0.';
    END IF;
    ```
8. 支持[FOR](/pql/for.md)和[WHILE](/pql/while.md)循环：

    ```sql
    FOR $id, $name IN (SELECT id, name FROM table3) LOOP
        PRINT $id;
        PRINT $name;
        SLEEP 1 SECOND;
    END LOOP;
    ```
9. 支持[请求数据接口](/pql/request.md)和[发送邮件](/pql/send.md)：

    ```sql
    REQUEST JSON API 'http://www.domain.com/api?id=1'
        PARSE '/data' AS TABLE;

    SEND MAIL "test mail"
        CONTENT "hello world"
        TO "user@domain.com";
    ```
10. 支持[Excel](/pql/excel.md)、[CSV](/pql/csv.md)、[TXT](/pql/txt.md)等文件的读写操作：

    ```sql
    OPEN mysql.db1;
        GET # SELECT * FROM table1;
    SAVE AS NEW EXCEL "example.xlsx"  USE TEMPLATE  "template.xlsx";
        PREP # INSERT INTO sheet1 (A, B, C) VALUES ('姓名', '年龄', '分数');
        PUT # INSERT INTO sheet1 ROW 2 (A, B, C) VALUES (id, '#name', &title);
    ``` 


PQL的最大的特点就是“简单”，可以在你的任何Java或Scala项目中使用，也可以保存为独立的脚本文件运行。更多更强大的功能请参阅各语句对应的文档。

---
参考链接

* [使用PQL](/pql/use-pql.md)
* [PQL基本语法](/pql/basic.md)
* [向控制台输出内容 PRINT](/pql/print.md)
* [数据源连接配置](/pql/properties.md)
* [打开和切换数据源 OPEN](/pql/open.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [中间缓存数据库 CACHE](/pql/cache.md)
* [PQL中的变量](/pql/variable.md)
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [PQL中的Json](/pql/json.md)
* [条件控制 IF](/pql/if.md)
* [条件控制 CASE](/pql/case.md)
* [循环遍历 FOR](/pql/for.md)
* [循环遍历 WHILE](/pql/while.md)
* [请求数据接口 REQUEST](/pql/request.md)
* [发送邮件 SEND](/pql/send.md)
* [输出Excel文件](/pql/excel.md)
* [输出CSV文件](/pql/csv.md)
* [输出文本文件](/pql/txt.md)