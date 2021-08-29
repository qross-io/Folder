# PQL 全局变量

与用户变量不同，用户变量的作用域仅在当前 PQL 过程或者控制语句内部，全局变量作用域是整个 PQL 环境，可以在同一个系统的任何 PQL 过程中使用。

```sql
PRINT @NOW; -- 打印当前时间，如 '2020-08-04 09:31:22'
```

**全局变量以`@`符号开头，全名规则与用户变量相同，变量名也不区分大小写，但字母建议全部大写。**

## 全局变量列表

* `@NOW` 当前时间，包含时分秒及毫秒等信息
* `@TODAY` 今天的日期，时分秒均为`0`
* `@YESTERDAY` 昨天的日期，时分秒均为`0`
* `@TOMORROW` 明天的日期，时分秒均为`0`
* `@TIMESTAMP` 当前时间的时间戳（秒）。

* `@ARGUMENTS` 可以获得传递给 PQL 的所有参数，这是一个数据行。

* `@KEEPER_IS_RUNNING` 判断 Keeper 是否在运行。
* `@KEEPER_HTTP_SERVICE` 查找可用的 Keeper Http 服务地址，包括 IP 地址和端口，如`192.168.1.1:7700`。如果 Keeper 在运行，则返回可用资源最多的那台机器。
* `@KEEPER_HTTP_ADDRESS` 可以设置一个常用的 Keeper Http 服务节点的 IP 地址。可以在[系统](/master/keeper/settings.md)中设置。
* `@KEEPER_HTTP_PORT` 可以设置 Keeper Http 服务端口，默认值为 7700。可以[系统](/master/keeper/settings.md)中设置。
* `@KEEPER_HTTP_TOKEN` 访问 Keeper Http 服务的 TOKEN，可以[系统](/master/keeper/settings.md)中设置。

* `@ROWS` 最后一个`SELECT`语句返回的记录数，包括[GET语句](/pql/get.md)和[PASS语句](/pql/pass.md)中的 SELECT 查询也会更新这个值
* `@COUNT_OF_LAST_SELECT` `@ROWS`的完整写法
* `@AFFECTED_ROWS` 最后一个非查询语句影响数据库的行数
* `@AFFECTED_ROWS_OF_LAST_NON_QUERY` `@AFFECTED_ROWS`的完整写法 
* `@COUNT` 最后一条`GET`语句获取的数据的行数
* `@COUNT_OF_LAST_GET` `@COUNT`的完整写法 
* `@TOTAL` 最近一组所有`GET`语句获取的数据的总行数
* `@TOTAL_COUNT_OF_RECENT_GET` `@TOTAL`的完整写法
* `@BUFFER` 使用`GET`语句后，缓冲区会保存记录，这个变量可以访问缓冲区，是一个二维数据表格
* `@AFFECTED_ROWS_OF_LAST_PUT` 最后一个`PUT`语句影响数据库的行数
* `@TOTAL_AFFECTED_ROWS_OF_RECENT_PUT` 最近一组所有`PUT`语句影响数据库的行数
* `@AFFECTED_ROWS_OF_LAST_PREP` 最后一条PREP语句影响数据库的行数

* `@USERID` 登录模式下可以获得用户ID
* `@USERNAME` 登录模式下可以获得用户名
* `@ROLE` 登录模式下可以获得用户的角色
* `@USER` 登录模式下用户的所有鉴权信息，是一个数据行。也可以通过属性名直接访问，如鉴权信息中包含`email`字段，则可以通过`@EMAIL`访问这个字段

* `@COOKIES` 可以访问 Cookie，详见[OneApi Cookies](/oneapi/cookies.md)
* `@SESSION` 可以访问 Session，详见[OneApi Session](/oneapi/session.md)
* `@LANGUAGE` 获得当前用户的默认界面语句，适用于多语言环境

* `@THEME` 获取一个随机主题颜色，是一个数据行，包含 3 个颜色字段`primary`、`lighter`和`darker`。

* `@LOCAL_IP` 获取本机的 IP 地址。
* `@RUNNING_DIR` 获取当前 jar 包的保存目录。

除了以上全局变量外，PQL 的全局设置的每一项都对应一个全局变量，如 `@QROSS_HOME`等，详见 [PQL 全局设置](/pql/setup.md)。

## 自定义全局变量

有时我们需要在不同的 PQL 过程之间使用相同的全局变量，但 PQL 内置的全局变量远远不能满足需求，PQL 提供了自定义全局变量的途径。

```sql
IF @LAST_UPDATE_TIME IS UNDEFINED THEN
    SET @LAST_UPDATE_TIME = '2020-01-01 00:00:00';
END IF;

GET # SELECT name, score FROM table1 WHERE update_time>=@LAST_UPDATE_TIME;
PUT # INSERT INTO table2 (name, score) VALUES (?, ?);
```

以上示例中，全局变量`@LAST_UPDATE_TIME`保存最后一次的更新时间，如果这个 PQL 过程每隔一段时间执行，则可以通过这种方式做到增量读取。定义和更新复合类型全局变量时需使用`=:`符号赋值或使用 [VAR 语句](/pql/var.md)。

**用户自定义的全局变量会有归属权问题：** 在登录状态下，定义的全局变量归属当前登录用户，即不同的用户可以使用相同名称的全局变量; 在默认状态下，定义的全局变量归属整个系统，任何人都可访问。用户定义的全局变量优先于系统全局变量。管理员可以通过 [Master 管理工具](/master/overview.md)查看和新增[自定义全局变量](/master/system/variables.md)。

## 全局变量嵌入到语句中

与用户变量不同，已嵌入到语句中的未声明的全局变量在计算时如找不到定义，将忽略，仅在控制台发出警告提醒。与用户变量的规则一样：如果遇到变量名与语句冲突不能正确识别时，可在变量名两端加上小括号，如`@(NOW)`; 如果不希望字符串变量有默认的引号时，可以变量末尾加上叹号，如`@NOW!`。 

---
参考链接

* [数据类型](/pql/datatype.md)
* [用户变量 $](/pql/variable.md)
* [调度变量 %](/pql/variable.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)