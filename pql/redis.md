# PQL 中的 Redis

PQL 中通过 **REDIS 语句**可以支持 Redis 的所有命令，与普通的 SQL 语句一样，每条语句均有返回值。这里不介绍 Redis 的每个命令，只通过一些示例了解 REDIS 语句的用法。

## 打开一个Redis数据库

```sql
OPEN REDIS "host" SELECT 1;
```

* `host`名称需要在 [properties 文件](/pql/properties.md) 中预先配置好。
* 如果只是使用配置文件中指定的默认数据库，可以省略 `SELECT`。但是如果在当前 PQL 过程中如果要切换数据库，不能只写`SELECT`部分，仍需要使用完整的 [OPEN 语句](/pql/open.md)进行切换。

## 使用 REDIS 语句

REDIS 语句和在控制台中输入的 Redis 命令的格式一致。

```sql
REDIS keys *;
REDIS set test 1;
REDIS hget test hello;
REDIS hgetall test;
```

* REDIS 语句必须以关键词`REDIS`开头，`REDIS`关键词后是标准 Redis 命令的内容。
* 每个 REDIS 命令都有返回值，大部分返回单值和列表，小部分为数据行，只有 **GEO** 数据类型的几个命令返回数据表。
* 返回数据行的命令有`HGETALL`, `ZRANGEBYSCORE`, `ZRANGE`, `ZREVRANGE`, `ZREVRANGEBYSCORE`。
* 返回数据表的命令有`GEOPOS`, `GEORADIUS`, `GEOHASH`，均为地理坐标相关。

## 和 SQL 语句一样的操作

同 SQL 语句一样，REDIS 语句一样可用于 [VAR 语句](/pql/var.md)和 [SET 语句](/pql/var.md)的右侧，单独使用时表示隐式输出，在 [OUTPUT 语句](/pql/output.md)中使用表示显示输出。在 [FOR 语句](/pql/for.md)中使用进行遍历，在 [GET 语句](/pql/get.md)中使用可以将数据保存在缓冲区，在 [PUT 语句](/pql/put.md)中使用表示批量更新。也可以用 [Sharp 表达式](/pql/sharp.md)进行结果再编辑，或者[嵌入变量和各种表达式](/pql/place.md)。

```sql
OPEN REDIS 'school';
    GET # REDIS hgetall students -> TO TABLE (name, score);
SAVE TO mysql.school;
    PUT # INSERT INTO students (name, score);
```

或

```sql
OPEN mysql.school;
    GET # SELECT name, score FROM students;
SAVE TO REDIS 'school';
    PREP # REDIS del students;
    PUT # REDIS hset students &name #score;
```

* 可以跨数据源进行数据传输，如上例 MySQL 和 Redis 之间。
* GET 语句会将 REDIS 语句返回的值自动转成数据表，也可以像上例一样手工转换并设置字段名。自动转换规则见 [GET 语句](/pql/get.md)。
* 使用`SAVE TO REDIS`设置目标 Redis 数据库，默认使用`OPEN REDIS`的源 Redis 数据库。
* [PREP 语句](/pql/prep.md)和 [PASS 语句](/pql/pass.md)都支持 Redis 操作。
* [PASS 语句](/pql/pass.md)和 [PUT 语句](/pql/put.md)中的 Redis 操作都会启动`Pipelined`模式，进行批量查询和更新。
* 多线程语句如 PAGE、BLOCK、BATCH 等语句不支持 Redis 操作。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [在目标数据源执行非查询操作 PREP](/pql/prep.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [更优雅的数据操作方法 Sharp 表达式](/pql/sharp.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [PQL 中的 SQL 语句](/pql/sql.md) 
* [循环遍历 FOR](/pql/for.md)