# 版本和更新

作者会一个月发布一个稳定版本。PQL 的最新稳定版本为 **v1.3.0**，发布时间 **2021-03-23**。

## v1.3.0 (2021.3.23)

这个版本已发布的中央仓库，地址`io.qross:pql:1.3.0`。“自定义系统函数”现已支持！

1. [自定义系统函数](/pql/global-function.md)现已可用，可以创建用于整个系统的函数或过程。
2. 新增一些[全局函数](/pql/global-function.md)，LPAD, LTRIM, RPAD, LTRIM, TRIM 等。
3. Marker 新增一些关于 [Markdown](/voyager/markdown.md) 的语法，如字体、行高等。
4. 读 CSV 文件已支持，读写现在均支持字符串类型，而不是简单的用逗号分隔。

## v1.2.0 (2021-02-27)

这个版本 PQL 并不是升级重点，只是配合其他组件的更新。

1. Debug 日志美化，可以在 HTML 页面上显示更美观的格式。
2. 新增多个[字符串函数](/pql/function-text.md)和几个[数字函数](/pql/funtion-numeric.md)。
3. [Worker 执行器](/pql/worker.md)现在支持`--log`参数，通过指定参数值`html`来输出 HTML 格式的日志。
4. [Markdown 扩展语法](/voyager/markdown.md)新增行内间距，例如`-| 20 |-`。
5. 新增两个与主题样式相关的全局变量`@THEME`和`@SPOTLIGHT`。

## v1.1.0 (2021-02-03)

本次升级使用新的版本号规则：第 1 位数字代表年份，`1`表示`2021年`；第 2 位数字代表月份，`1`表示`1月`。第 3 位数字表示开发过程中的小版本号。本次升级内容如下：

1. [全局函数](/pql/gloabl-function.md)逻辑已加入，第一版只增加了几个函数。
2. Voyager 现在支持 [Markdown 文件](/voyager/markdown.md)，扩展名为`.md`。
3. Voyager 增加[母版页](/voyager/master.md)，使用`<# page template="/template/form.html?key=value" />` 引入母版页。
4. Voyager 增加[设置项](/voyager/setup.md)：静态站点`voyager.static.site`和图库站点`voyager.gallery.site`，默认空值，即当前站点。
5. OPEN 语句现在可以直接通过连接串打开数据库，见 [OPEN DATABASE](/pql/open.md)。
6. SAVE 语句现在也可以直接通过连接串打开数据库，见 [SAVE AS/TO DATABASE](/pql/save.md)。
7. [RUN 语句](/pql/run.md)现在支持运行 PQL 文件，格式
    ```sql
    RUN PQL '/abc/test.sql?a=1&b=2&c=3';
    ```
    可将参数附在地址后面，中间使用空格隔开，地址和参数两边的引号可省略，但不可包含空格。
8. 新增 [PAR 语句](/pql/par.md)，用于多线程并行运算。
    ```sql
    PAR # RUN COMMAND 'java -jar test.jar';
    ```
9. 新增 [LOAD 语句](/pql/load.md)，用于在程序运行时手动加载数据连接配置。
    ```sql
    LOAD YML FROM NACOS 'localhost:8848:io.qross.config:connections';
    ```
10. [数据源配置](/pql/properties.md)现在不仅只支持`properties`文件，也支持从 Yaml 文件、Nacos 配置中心或从其他服务的 URL 接口获取。
11. [Sharp 表达式](/pql/sharp-table.md)新增`WHERE`和`DELETE`用于表格数据过滤或删除。
12. [Marker 应用](/voyager/markdown.md)现在支持文字样式，如`/green,16:绿色标准字体/`。
13. [Marker 应用](/voyager/markdown.md)现在支持行间隔设置，如`-- 20 --`表示在上下两行之间设置一个`20`像素的间隔。
14. `%`现在也是特殊字符，如遇到字符冲突时使用用`~u0025`代替，一般情况下不需要使用。
15. [Worker 执行器](/pql/worker.md)现在支持将参数写在`--file`参数指定的文件名后面。

## v0.6.5 (2020-12-24)

本次版本升级较少，主要是修复 Bug。

1. [FILE](/pql/file.md) 和 [DIR](/pql/dir.md) 的重命名、复制和移动操作逻辑已实现。
2. [Voyager 模板引擎](/voyager/overview.md) 增加说明文档。
3. PQL 类一些方法更新。

## v0.6.4 (2020-09-17)

从这个版本起，开始补充各个产品的文档。

1. PQL 和 OneApi 文档已完成，可以在官网找到。
2. [Marker 应用](/pql/marker.md)加入，可以将 **Markdown** 文档转成 **HTML**。
3. [自定义用户函数](/pql/function.md)升级，现在已不仅作为语句复用目的。
    * 自定义函数现在可以返回值了。
        ```sql
        FUNCTION $plus($a, $b)
            BEGIN
                RETURN $a + $b;
            END;
        ```
    * [RETURN 语句](/pql/return.md)可以将函数的结果返回，也可以中断并返回整个PQL过程的结果。
    * 自定义函数已支持嵌入到语句中。
        ```sql
        SELECT * FROM table WHERE socre>=$pass(60);
        ```    
4. [有返回值的语句](/pql/evaluate.md)重新整理，这些语句可以用在很多地方。
    * [INVOKE语句](/pql/invoke.md)不仅可以返回值，而且可以访问Java静态属性了。
    * [RUN语句](/pql/run.md)现在可以获取运行日志和错误日志。
    * [SEND语句](/pql/send.md)现在返回发送日志。
    * [EXEC语句](/pql/exec.md)逻辑重新整理。
    
5. [集合对象的属性和索引访问](/pql/collection.md)支持升级，现在不仅数据行支持属性访问，数组和数据表也已支持。如`$list[2]`、`$table.first['id']`等。
6. Sharp 表达式更新。
    * 新增[正则表达式](/pql/sharp-regex.md)的多个操作，如`FIND FIRST IN`等。
    * [日期时间](/pql/sharp-datetime.md)单位新增`NANO`、`MICRO`、`NANOS`、`MICROS`。
    * [数学运算](/pql/sharp-numeric.md)新增取余`MOD`、`MIN`、`MAX`。
    * [数组操作](/pql/sharp-array.md)中`ADD`可以一次添加多个值，如`ADD 1,2,3`; 排序操作`SORT`、`ASC`、`DESC`已加入。
    * [数据行操作](/pql/sharp-row.md)和[数据表操作](/pql/sharp-table.md)加入`HAS`，用来判断是否包含某个字段。
    * [比较操作](/pql/sharp-if.md)加入：`LESS THAN`、`LESS THAN OF EQUALS`、`GREATER THAN`、`GREATER THAN OR EQUALS`。
7. 多条语句优化。

本次不兼容修改主要涉及 [Sharp 表达式](/pql/sharp.md)

* 时间单位`MILLISECONDS`已移除，使用`MILLI`或`MILLIS`代替
* 数字操作`PERCENT`修改为`TO PERCENT`
* 字符中操作`WHOLE MATCHES`移除
* 字符串操作`TAKE BEFORE FIRST`和`TAKE AFTER FIRST`移除 
* 字符串操作`SUBSTRING`移除，保留`SUBSTR`
* 数据表操作`TO MAP`修改为`TO NESTED MAP`
* 数组操作`HEAD`修改为`FIRST`

## v0.6.3 (2020-08-16)

`0.6.3`版本除了 Bug 和优化之外，主要更新内容如下：

1. 增加了对[Redis支持](/pql/redis.md)。
   * 示例：`REDIS GET keyname;`
   * 使用 `OPEN REDIS 'host'` 语句连接一个Redis服务器。
   * 支持REDIS的所有查询和更新命令。
   * Redis命令返回值根据命令名确定，大部分返回单值和列表，仅部分`HASH`和`SortedSet`类型的命令返回数据行，仅`GEO`类型中有的命令返回数据表。
   * 基本上支持SELECT语句的地方都支持REDIS语句，如[SET](/pql/set.md)、[FOR](/pql/for.md)、[OUTPUT](/pql/output.md)等。
   * 可以像其他语句一样嵌入变量和表达式。
   * 支持[GET](/pql/get.md)和[PUT](/pql/put.md)操作，但不支持多线程操作，如[PAGE](/pql/page.md)、[BATCH](/pql/batch.md)等。
2. [OneApi](/oneapi/overview.md) 升级，支持身份认证
   * 支持[Token 认证](/oneapi/token.md)、用户认证和[动态Token认证](/oneapi/token.md)。
   * 可通过配置文件或 Qross 系统进行[相关设置](/oneapi/setup.md)。
   * 新增[管理功能](/oneapi/management.md)相关的方法，可自己实现接口。
3. [RUN SHELL](/pql/run.md)语句升级，现在已支持分号、管道符和引号。
4. [FILE 语句](/pql/file.md)新增READ和WRITE功能，用于读取整个文件或附加内容。
5. 原生 SHOW 语句已支持。
6. [IF](/pql/if.md) 和 [CASE](/pql/case.md) 短语句已经像 FOR 语句一样支持查询语句。如
    ```sql
    VAR $table := 
        IF $i == 1 THEN
            (SELECT * FROM table1)
        ELSE
            (SELECT * FROM table2)
        END;
    ```
7. 非查询语句现在也支持数据结果再加工。如
    ```sql
    SET $count := UPDATE table SET a=1 -> IF ZERO 1;
    ```
8. 更强大的 [GET 语句](/pql/get.md)，现在可以在GET语句添加任何类型的数据，如：
    ```sql
    GET # [1, 2, 3, 4, 5];
    PUT # INSERT INTO table1 (score) VALUES (#item);
    ```
9. 由于数据处理过程中有的值是 Json 字符串，新增了[操作 Json 字符串](/pql/sharp-json.md)的方法。
    ```sql
    SET $json := '{ "id": 1, "name": "Tom" }';
    PRINT ${ $json FIND '/name' AS VALUE }; 
    ```
10. [Sharp 表达式](/pql/sharp.md)新增多个处理方法。

以下内容不再向下兼容

* Sharp 表达式中字符串操作`INIT CAP`修改为`CAPITALIZE`

## 0.6.2 及以下

旧版本更新记录已归档。

---
参考链接

* [PQL 概览](/pql/overview.md)
* [OneApi 统一接口](/oneapi/overview.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [DataHub 数据开发框架](/datahub/overview.md)

