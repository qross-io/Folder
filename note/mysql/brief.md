# MySQL 简介

MySQL 是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

MySQL 是一种关系型数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

MySQL 所使用的 SQL 语言是用于访问数据库的最常用标准化语言。MySQL 软件采用了双授权政策，分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择 MySQL 作为网站数据库。

MySQL 最常用版本是 5.7 和 8.0，MariaDB 是 MySQL 的另一个开源分支。很多数据库和数据仓库都使用 MySQL 作为连接器，如 TiDB、AanalyticDB、Doris 等。

## 安装 MySQL

...

## 使用命令行连接

```shell
mysql -uroot -pdiablo
mysql -h localhost -P 3306 -D mydb  -u root -pDiablo
```

命令行连接需要在 Linux 环境下或者在本地安装连接器，可选图形界面管理工具 DBeaver、WorkBench、NaviCat等。

## 使用 JDBC 连接

MySQL 5.7 的 JDBC Driver 为 `com.mysql.jdbc.Driver`，MySQL 8.0 版本的驱动为 `com.mysql.cj.jdbc.Driver`。

MySQL 的连接串样式为 `jdbc:mysql://localhost:3306/mydb?user=root&password=password&useUnicode=true&characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true`

* `3306`是 MySQL 的默认端口号
* `allowPublicKeyRetrieval=true` 这个参数是 MySQL 8 专用，以允许客户端从服务器获取公钥；但是需要注意的是可能会导致恶意的代理通过中间人攻击(MITM)获取到明文密码，所以默认是关闭的，必须显式开启

## MySQL 元数据信息

MySQL 的元数据存储在`information_schema`库中，其中表`SCHEMATA`表中保存所有的数据库信息，`TABELS`表中保存所有的数据表信息，`COLUMNS`中保存所有的数据字段信息。以下为查询示例：

```sql
SET $not_in := "('information_schema', 'dbhealth', 'mysql', 'performance_schema', 'binlog_', 'test', 'sys', 'sakila')";
SELECT schema_name AS database_name, schema_name FROM information_schema.SCHEMATA WHERE schema_name NOT IN $not_in!;
SELECT table_schema AS database_name, table_schema, table_name, table_rows, table_comment, auto_increment, DATE_FORMAT(create_time, '%Y-%m-%d %H:%i:%s') AS create_time, DATE_FORMAT(update_time, '%Y-%m-%d %H:%i:%s') AS update_time FROM information_schema.TABLES WHERE table_schema NOT IN $not_in! AND table_name<>'schema_version';
SELECT table_schema AS database_name, table_schema, table_name, column_name, left(column_default,100) AS column_default, column_type, column_comment FROM information_schema.COLUMNS WHERE table_schema NOT IN $not_in! AND table_name<>'schema_version';
```

---
参考链接

* [MySQL 数据类型](/note/mysql/datatype.md)
* [MySQL 中的 SQL 语句](/note/mysql/sql.md)
* [MySQL 函数](/note/mysql/function.md)
* [MySQL 索引](/note/mysql/index.md)
