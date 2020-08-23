# 版本和更新
作者会每周发布一个预览版本的更新，一到两个月会发布一个稳定版本。PQL的最新稳定版本为 **v0.6.3-2-RELEASE**，最新快照版本是 **v0.6.4-21-SNAPSHOT**。 

## v0.6.4（开发中）
1. [自定义用户函数](/pql/function.md)升级，现在已不仅作为语句复用目的。
    * 自定义函数现在可以返回值了。
        ```sql
        FUNCTION $plus($a, $b)
            BEGIN
                RETURN $a + $b;
            END;
        ```
    * [RETURN语句](/pql/return.md)可以将函数的结果返回，也可以中断并返回整个PQL过程的结果。
    * 自定义函数已支持嵌入到语句中。
        ```sql
        SELECT * FROM table WHERE socre>=$pass(60);
        ```    
2. [集合对象的属性和索引访问](/pql/collection.md)支持升级，现在不仅数据行支持属性访问，数组和数据表也已支持。如`$list[2]`、`$table.first['id']`等。
3. Sharp表达式更新。
4. 多条语句优化。


## v0.6.3
`0.6.3`版本相比`0.6.2`除了Bug和优化之外，主要更新内容如下：
1. 增加了对[Redis支持](/pql/redis.md)。
   * 示例：`REDIS GET keyname;`
   * 使用 `OPEN REDIS 'host'` 语句连接一个Redis服务器。
   * 支持REDIS的所有查询和更新命令。
   * Redis命令返回值根据命令名确定，大部分返回单值和列表，仅部分`HASH`和`SortedSet`类型的命令返回数据行，仅`GEO`类型中有的命令返回数据表。
   * 基本上支持SELECT语句的地方都支持REDIS语句，如[SET](/pql/set.md)、[FOR](/pql/for.md)、[OUTPUT](/pql/output.md)等。
   * 可以像其他语句一样嵌入变量和表达式。
   * 支持[GET](/pql/get.md)和[PUT](/pql/put.md)操作，但不支持多线程操作，如[PAGE](/pql/page.md)、[BATCH](/pql/batch.md)等。
2. [OneApi](/oneapi/overview.md)升级，支持身份认证
   * 支持[Token认证](/oneapi/token.md)、用户认证和[动态Token认证](/oneapi/token.md)。
   * 可通过配置文件或Qross系统进行[相关设置](/oneapi/setup.md)。
   * 新增[管理功能](/oneapi/management.md)相关的方法，可自己实现接口。
3. [RUN SHELL](/pql/run.md)语句升级，现在已支持分号、管道符和引号。
4. [FILE语句](/pql/file.md)新增READ和WRITE功能，用于读取整个文件或附加内容。
5. 原生SHOW语句已支持。
6. [IF](/pql/if.md)和[CASE](/pql/case.md)短语句已经像FOR语句一样支持查询语句。如
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
8. 更强大的[GET语句](/pql/get.md)，现在可以在GET语句添加任何类型的数据，如：
```sql
    GET # [1, 2, 3, 4, 5];
    PUT # INSERT INTO table1 (score) VALUES (#item);
```
9. 由于数据处理过程中有的值是Json字符串，新增了[操作Json字符串](/pql/sharp-json.md)的方法。
```sql
SET $json := '{ "id": 1, "name": "Tom" }';
PRINT ${ $json FIND '/name' AS VALUE }; 
```
10. [Sharp表达式](/pql/sharp.md)新增多个处理方法。

以下内容不再向下兼容
* Sharp表达式中字符串操作`INIT CAP`修改为`CAPITALIZE`

---
参考链接
* [Redis语句](/pql/redis.md)
* [Sharp表达式](/pql/sharp.md)
* [OneApi统一接口](/oneapi/overview.md)
* [运行Shell命令 RUN](/pql/run.md)
* [文件操作 FILE](/pql/file.md)
* [条件控制 IF](/pql/if.md)
* [条件控制 CASE](/pql/case.md)
* [将数据保存到缓存区 GET](/pql/get.md)
* [Sharp表达式](/pql/sharp.md)
* [Sharp表达式 - Json字符串](/pql/sharp-json.md)