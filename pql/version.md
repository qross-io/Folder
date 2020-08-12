# 版本和更新
作者会每周发布一个预览版本的更新，一到两个月会发布一个稳定版本。PQL的最新预览版本是 **v0.6.3-68-SNAPSHOT**，最新稳定版本为 **v0.6.2-18-RELEASE**。  
0.6.3版本除了Bug和优化之外，主要更新内容如下：
1. 增加了对Redis支持。  
   * 示例：REDIS GET keyname;
   * 使用 OPEN REDIS 'host' 语句连接一个Redis服务器。
   * 支持REDIS的所有查询和更新命令。
   * Redis命令返回值根据命令名确定，大部分返回单值和列表，仅部分HASH和SortedSet类型的命令返回数据行，仅GEO类型中有的命令返回数据表。
   * 基本上支持SELECT语句的地方都支持REDIS语句，如SET、FOR、OUTPUT等。
   * 可以像其他语句一样嵌入变量和表达式。
   * 支持GET和PUT操作，但不支持多线程操作，如PAGE、BATCH等。
2. OneApi升级，支持身份认证
   * 支持Token认证、用户认证和动态Token认证。
   * 可通过配置文件或Qross系统进行配置。
   * 新增管理功能接口。
3. RUN SHELL语句升级，现在已支持分号、管道符和引号。
4. FILE语句新增READ和WRITE功能，用于读取整个文件或附加内容。
5. 原生SHOW语句已支持。
6. IF和CASE短语句已经像FOR语句一样支持查询语句。如
```
    VAR $table := 
        IF $i == 1 THEN
            (SELECT * FROM table1)
        ELSE
            (SELECT * FROM table2)
        END;
```
7. 非查询语句现在也支持数据结果再加工。如
```
    SET $count := UPDATE table SET a=1 -> IF ZERO 1;
```
8. 更强大的GET语句，现在可以在GET语句添加任何类型的数据，如：
```
    GET # [1, 2, 3, 4, 5];
    PUT # INSERT INTO table1 (score) VALUES (#item);
```
9. 由于数据处理过程中有的值是Json字符串，新增了操作Json字符串的方法。
```
SET $json := '{ "id": 1, "name": "Tom" }';
PRINT ${ $json FIND '/name' AS VALUE }; 
```
10. Sharp表达式新增多个处理方法。