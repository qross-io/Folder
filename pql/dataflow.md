# 极致简单的数据流转操作

在数据计算和开发过程中，把数据从一种形式转变成另一种形式是最最经常的操作，中间有可能涉及到一个数据源也可能涉及到多个数据源。PQL 提供了一整套解决方案，解决数据在同一数据源或不同数据源之间的流转问题。

```sql
OPEN mysql.classes;
    GET # SELECT name, score, grade FROM students;
SAVE TO mysql.school;
    PUT # INSERT INTO scores (name, score) VALUES (&name, #score);
SAVE AS CSV FILE 'd:/scores.csv' WITH HEADERS (name AS 姓名, score AS 分数);
```

上例中，从`mysql.classes`库查询数据，然后保存到`mysql.school`数据库中，最后再将数据保存一份 CSV 文件。不知道在 Java 或 Python 里完成以上工作需要多少代码，但是在 PQL 中，就只需要这几行代码。

## 数据流转的原理

对于每一次数据流转操作，都分为三个组成部分：

1. **数据源**，表示从哪里的获取数据，数据源可以是一个，也可以是多个。可以从数据获取一次数据，也可以获取多次数据。数据源可以是关系型数据库、文件、中间缓存数据库或者变量或常量等等。
2. **中间缓存区**，PQL会把从数据源获取的数据保存在缓存区，缓冲区本质上是一个二级表格。多次获取数据会多次向缓冲区插入数据。缓冲区会在一次 PUT 动作之后，标记为待清除，会在下一次 GET 动作之前清除。
3. **数据目的地** 是指将数据保存或更新到哪里，数据目的可以是关系型数据库，也可以是中间缓存数据库，还可以是文件等可以保存数据的地方。

中间缓存区在内存中，每次流转操作就是通过 GET 操作把数据先放在缓存区，然后再通过 PUT 操作更新到数据目的地。

## 数据流转的参与者

除了中间缓存区之外，数据源和数据目的可以理解为是数据流转的参与者，主要包括：

* 任何关系型数据库，支持 GET 操作和 PUT 操作。
* [Redis](/pql/redis.md)，支持 GET 和 PUT 操作。
* [中间内存数据库](/pql/cache.md)，因为中间缓存区只是一张表，当需要保存多张表时，可使用中间内存数据库。支持 GET 操作和 PUT 操作。
* [中间文件数据库](/pql/temp.md)，可以保存大量的中间数据。支持 GET 操作和 PUT 操作。
* 文件，如 [Excel](/pql/excel.md)、[CSV](/pql/csv.md)、[TXT](/pql/txt.md)等。支持 PUT 操作，有限支持 GET 操作。
* [Json 接口](/pql/request.md)或字符串，支持 GET 操作。
* 变量或常量，支持 GET 操作，会自动转成表格。


## 数据流转的主要操作

上面已经提到了 GET 操作和 PUT 操作，也是数据流转中最常用的两个。数据流转的操作有：

* [OPEN 操作](/pql/open.md)，切换数据源。
* [GET 操作](/pql/get.md)，将数据从数据源保存到缓存区。
* [SAVE 操作](/pql/save.md)，切换数据目的地或直接将数据保存。
* [PUT 操作](/pql/put.md)，将缓存区的数据保存到数据目的，一般是关系型数据库。
* [PASS](/pql/pass.md)，用缓存区的数据作为基础再进行一次查询。
* [CACHE 操作](/pql/cache.md)，将数据源的数据保存到中间内存数据库。
* [TEMP 操作](/pql/temp.md)，将数据源的数据保存到中间文件数据库。
* [PAGE 操作](/pql/page.md)，大数据量场景下分页读取关系型数据库的数据源。
* [BLOCK 操作](/pql/block.md)，大数据量场景下分块读取关系型数据库的数据源。
* [PROCESS 操作](/pql/process.md)，大数据量场景下对 PAGE 或 BLOCK 操作的数据再进行一次查询。
* [BATCH 操作](/pql/batch.md)，大数据量场景下将 PAGE 或 BLOCK 或 PROCESS 操作的数据保存到数据目的地。

除 OPEN 和 SAVE 语句之外，其他的操作都表现为 `操作名+参数+连接符#号+语句`，如 CACHE 操作：

```sql
CACHE 'table1' # SELECT * FROM students;
```

每一个操作的详细功能，详见每一个语句的说明。合理利用每一种操作，可以在数据处理中事半功倍。关于数据流转的模式总结，见 [BATCH 语句](/pql/batch.md)最后一节的内容。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [使用缓冲区数据再查询 PASS](/pql/pass.md)
* [将缓冲区的数据保存到数据库 PUT](/pql/put.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的数据加工 PROCESS](/pql/process.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)