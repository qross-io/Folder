# PQL 一种简单优雅的数据处理语言
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
    PUT # INSERT INTO sheet1 ROW 2 (A, B, C) VALUES (id, ‘#name’, &title);
``` 

PQL的最大的特点就是“简单”，可以在你的任何Java或Scala项目中使用，也可以保存为独立的脚本文件运行。更多更强大的功能请参阅各语句对应的文档。

# PQL 应用

### 统一接口 OneApi
**OneApi** 是PQL的一种应用，可以通过PQL编辑Web服务的接口，以帮助后端工程师减少大量的重复代码编写工作，快速实现业务逻辑。请参阅[OneApi文档](/oneapi/overview.md)获取更多信息。

### 模板引擎 Voyager
**Voyager** 是PQL的另一个应用，和OneApi一样，也是应用于Web开发。其同类是FreeMarker或Thymeleaf，不过更简单直接，不需要记各种标记语法，直接在网页HTML代码中写SQL语句进行查询并输出数据。请参阅[Voyager文档](/master/overview.md)获取更多信息。

### 任务调度工具 Keeper
**Keeper** 是一个轻量级但功能强大的任务调度工具，支持运行PQL任务，也支持通过PQL扩展Keeper，如事件、依赖等。请参阅[Keeper文档](/keeper/overview.md)获取更多信息。

### 数据管理平台 Master
**Master** 是一个综合各种数据开发相关功能的管理平台，可以在上面编写PQL并直接运行、部署调度任务、管理服务接口等。请参阅[Master文档](/master/overview.md)获取更多信息。

# PQL 文档目录

* PQL快速入门
    + [PQL概览](/pql/overview.md)
    + [使用PQL](/pql/use-pql.md)
    + [数据源配置](/pql/properties.md)
    + [PQL源码和示例项目](/pql/example.md)
    + [版本和更新](/pql/version.md)

* PQL基础
    + [PQL基本语法](/pql/basic.md)
    + [打开和切换数据源 OPEN](/pql/open.md)
    + [PQL中的SQL语句](/pql/sql.md)    
    + [PQL数据类型](/pql/datatype.md)
    + [使用注释](/pql/comment.md)
    + [用户变量](/pql/variable.md)
    + [全局变量](/pql/global.md)
    + [向PQL过程传递参数](/pql/params.md)
    + [访问集合类型元素](/pql/collection.md)

* 数据流转
    + [跨数据源数据流转 SAVE](/pql/save.md)
    + [将数据保存在缓冲区 GET](/pql/get.md)    
    + [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
    + [在目标数据源执行非查询操作 PREP](/pql/prep.md)
    + [使用缓冲区数据再查询 PASS](/pql/pass.md)
    + [中间临时内存数据库 CACHE](/pql/cache.md)
    + [中间临时文件数据库 TEMP](/pql/temp.md)
    + [大数据量下的分页查询 PAGE](/pql/page.md)
    + [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
    + [大数据量下的数据加工 PROCESS](/pql/process.md)
    + [大数据量下的批量更新 BATCH](/pql/batch.md)

* 输出文件
    + [将数据保存到Excel文件](/pql/excel.md)
    + [将数据另存为CSV文件](/pql/csv.md)
    + [将数据另存为文本文件](/pql/txt.md)
    + [将数据另存为Json数据文件](/pql/json-file.md)

* PQL中的语句
    + [变量声明 SET](/pql/set.md)
    + [变量声明 VAR](/pql/var.md)
    + [在控制台输出信息 PRINT](/pql/print.md)
    + [返回结果 OUTPUT](/pql/output.md)
    + [集合类型数据编辑 LET](/pql/let.md)
    + [简单输出 ECHO](/pql/echo.md)
    + [启用调试 DEBUG](/pql/debug.md)
    + [执行字符串形式的语句 EXEC](/pql/exec.md)
    + [休息一下 SLEEP](/pql/sleep.md)
    + [退出程序 EXIT CODE](/pql/exit-code.md)  

* 分支和循环
    + [条件控制 IF](/pql/if.md)
    + [条件控制 CASE](/pql/case.md)
    + [条件表达式](/pql/condition.md)
    + [循环遍历 FOR](/pql/for.md)
    + [条件循环 WHILE](/pql/while.md)
    + [退出循环 EXIT](/pql/exit.md)
    + [继续下一次循环 CONTINUE](/pql/continue.md)

* 更优雅的数据操作
    + [Sharp表达式](/pql/sharp.md)
    + [字符串操作 TEXT](/pql/sharp-text.md)
    + [数字操作 INTEGER/DECIMAL](/pql/sharp-numeric.md)
    + [日期时间操作 DATETIME](/pql/sharp-datetime.md)
    + [正则表达式操作 REGEX](/pql/sharp-regex.md)
    + [数组操作 ARRAY](/pql/sharp-array.md)
    + [数据行操作 ROW](/pql/sharp-row.md)
    + [数据表操作 TABLE](/pql/sharp-table.md)
    + [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)
    + [数据判断](/pql/sharp-if.md)    

* PQL高级特性
    + [嵌入式查询表达式](/pql/query.md)
    + [PQL中的Json](/pql/json.md)
    + [富字符串](/pql/rich.md)

* 自定义函数
    + [函数声明 FUNCTION](/pql/function.md)
    + [中断数据并返回值 RETURN](/pql/return.md)
    + [函数调用 CALL](/pql/call.md)    

* 扩展操作
    + [请求Json数据接口 REQUEST](/pql/request.md)
    + [解析Json接口返回的数据 PARSE](/pql/parse.md)
    + [访问Redis](/pql/redis.md)
    + [发送邮件 SEND](/pql/send.md)
    + [文件操作 FILE](/pql/file.md)
    + [文件夹操作 DIR](/pql/dir.md)

* 其他语言相关
    + [在PQL中调用Java方法 INVOKE](/pql/invoke.md)
    + [在PQL中运行Shell命令 RUN](/pql/run.md)
    + [PQL中的Javascript](/pql/javascript.md)    

* 附录
    + [完整的嵌入规则表](/pql/place.md)
    + [特殊字符表](/pql/characters.md)
    + [io.qross.pql.PQL 类](/pql/class.md)
    + [PQL全局设置](/pql/setup.md)

# PQL 支持

PQL语言免费使用，有任何问题均可联系作者或留言。PQL的更新频率为每周一次。

**官方网站 [www.qross.io](http://www.qross.io)**  
**作者邮箱 [wu@qross.io](mailto:wu@qross.io)**


