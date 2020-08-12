# PQL中的Redis
PQL中通过REDIS语句支持REDIS的所有命令，与普通的SQL语句一样，每条语句均有返回值。这里不介绍Redis的每个命令，只通过一些示例了解REDIS语句的用法。

#### 打开一个Redis数据库
```
    OPEN REDIS "host" SELECT 1;
```
* `host`名称需要在[properties文件](/doc/pql/properties)中预先配置好。
* 如果只是使用配置文件中指定的默认数据库，可以省略`SELECT`。但是如果在当前PQL过程中如果要切换数据库，不能只写`SELECT`部分，仍需要使用完整的OPEN语句进行切换。

#### 使用REDIS语句
REDIS语句和你在控制台中输入的Redis命令的格式一致。
```
    REDIS keys *;
    REDIS set test 1;
    REDIS hget test hello;
    REDIS hgetall test;
```
* REDIS语句必须以关键词`REDIS`开头，`REDIS`关键词后是标准Redis命令的内容。
* 每个REDIS命令都有返回值，大部分返回单值和列表，小部分为数据行，只有GEO数据类型的几个命令返回数据表。
* 返回数据行的命令有`HGETALL`, `ZRANGEBYSCORE`, `ZRANGE`, `ZREVRANGE`, `ZREVRANGEBYSCORE`。
* 返回数据表的命令有`GEOPOS`, `GEORADIUS`, `GEOHASH`，均为地理坐标相关。

#### 和SQL语句一样的操作
同SQL语句一样，REDIS语句一样可用于[VAR语句](/doc/pql/var)和[SET语句](/doc/pql/var)的右侧，单独使用时表示隐式输出，在[OUTPUT语句](/doc/pql/output)中使用表示显示输出。在[FOR语句](/doc/pql/for)中使用进行遍历，在[GET语句](/doc/pql/get)中使用可以将数据保存在缓冲区，在[PUT语句](/doc/pql/put)中使用表示批量更新。也可以用[Sharp表达式](/doc/pql/sharp)进行结果再编辑，或者[嵌入变量和各种表达式](/doc/pql/embedded)。

```
    OPEN REDIS 'school';
        GET # REDIS hgetall students -> TO TABLE (name, score);
    SAVE TO mysql.school;
        PUT # INSERT INTO students (name, score);
```
或
```
    OPEN mysql.school;
        GET # SELECT name, score FROM students;
    SAVE TO REDIS 'school';
        PREP # REDIS del students;
        PUT # REDIS hset students &name #score;
```

* 可以跨数据源进行数据传输，如上例MySQL和Redis之间。
* GET语句会将REDIS语句返回的值自动转成数据表，也可以像上例一样手工转换并设置字段名。自动转换规则见[GET语句](/doc/pql/get)。
* 使用`SAVE TO REDIS`设置目标Redis数据库，默认使用`OPEN REDIS`的源Redis数据库。
* [PREP语句](/doc/pql/prep)和[PASS语句](/doc/pql/pass)都支持Redis操作。
* [PASS语句](/doc/pql/pass)和[PUT语句](/doc/pql/put)中的Redis操作都会启动`Pipelined`模式，进行批量查询和更新。
* 多线程语句如PAGE、BLOCK、BATCH等不支持Redis操作。

---
参考链接
* [打开和切换数据源 OPEN](/doc/pql/open)
* [跨数据源数据流转 SAVE](/doc/pql/save)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [缓冲区数据再查询 PASS](/doc/pql/pass)
* [在目标数据源执行非查询操作 PREP](/doc/pql/prep)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [中间缓存内存数据库 CACHE](/doc/pql/cache)
* [中间临时文件数据库 TEMP](/doc/pql/temp)
* [更优雅的数据操作方法 Sharp表达式](/doc/pql/sharp)
* [变量声明 SET](/doc/pql/set)
* [变量声明 VAR](/doc/pql/var)
* [PQL中的SQL语句](/doc/pql/sql) 
* [循环遍历 FOR](/doc/pql/for)