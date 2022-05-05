# 2021 年版本和更新

这里汇总了 2021 年的所有更新。

## v1.10.0 (2021.10.31)

1. [系统函数](/pql/global-function.md)新增一些[数字聚合运算](/pql/function-numeric.md)，如加和、均值等。
2. [系统函数](/pql/global-function.md)新增[正则表达式函数](/pql/funcion-text.md)。
3. [Sharp 表达式](/pql/sharp.md) 新增一些[数字聚合运算操作](/pql/sharp-numeric.md)，如方差、标准差等。

## v1.9.0（2021.9.29）

1. [系统函数](/pql/global-function.md) 新增一些[数字相关函数](/pql/function-numberic.md) 支持。
2. [Sharp 表达式](/pql/sharp.md) 新增一些[数字相关的操作](/pql/sharp-numeric.md)。

## v1.8.0（2021.8.29）

1. [系统函数](/pql/global-function.md) 新增[日期时间函数](/pql/function-datetime.md) 支持。
2. [Sharp 表达式 - 日期时间](/pql/sharp-datetime.md) 新增`GET WEEK OF YEAR`、`GET QUARTER`等 7 个操作。
2. [Sharp 表达式 - 字符串](/pql/sharp-text.md) 新增`PREFIX`和`SUFFIX`用于为字符串添加前缀或后缀。
3. [Sharp 表达式 - 字符串](/pql/sharp-text.md) `DROP`和`DROP RIGHT`现在参数支持字符串。
4. [Sharp 表达式 - 数字](/pql/sharp-numeric.md) `IF GREATER THEN ZERO`修改为`IF POSITIVE`，`IF LESS THEN ZERO`修改为`IF NEGATIVE`。
5. [DateTime 日期时间类](/datahub/datetime.md) 增加季度、月份、星期名称相关的方法。

## v1.7.0 (2021.7.28)

1. [CACHE 语句](/pql/cache.md)和 [TEMP 语句](/pql/temp.md)也支持 [GET 语句](/pql/get.md)的各种特性。
2. SELECT 语句和非查询语句现在加入参数检查，缺少参数时会报错。
3. 新的[数据表相关 Sharp 表达式](/pql/sharp-table.md)`TO GROUPS`，可以将数据表分组成多个数据表，用于接口开发。
4. 一些优化和 Bug 修复。

## v1.6.0 (2021.6.28)

这个版本以稳定为主，主要是代码优化和 Bug 修复工作。

1. 新的 [Sharp 表达式](/pql/sharp-table.md)，`INSERT IF NOT EXISTS`、`UPDATE`、`UPDATE IF NULL`、`UPDATE IF EMPTY`、`UPDATE IF UNDEFINED`，用于数据表操作。
2. 新的 [Sharp 表达式](/pql/sharp-array.md) `MAP`，用于列表的数据映射。
3. 新的全局变量`@ARGUMENTS`，可以获得传递给 PQL 过程的所有参数。
4. 新的全局变量`@KEEPER_HTTP_SERVICE`，查找可用的 Keeper HTTP 服务地址。
5. 新的全局变量`@KEEPER_IS_RUNNING`，判断 Keeper 是否正在运行。
6. 多项优化。

## v1.5.0 (2021.5.28)

这个版本主要是配合 Keeper 进行相关的更新。

1. 新的变量类型[调度作业变量](/keeper/job-variable.md)，作用域仅限于指定调度作业的所有任务实例。
2. 新的[数据类型](/pql/datatype.md) **哈希列表 HASHSET**，可以通过数组转换，哈希列表中的元素值不会重复。
3. Voyager 模板引擎已支持[逻辑前置](/voyager/syntax.md)，以添加在页面解析最前面执行的逻辑。
4. Voyager 模板引擎现在支持以 Json 格式[传递参数](/voyager/query.md)。
5. 现在 Voyager 模板引擎仅在 Spring Boot 页面开发时支持多语言，其他应用场景中与多语言相关的支持已移除。
6. 嵌入式 PQL 现在支持任意类型的文件模板，并且优化处理过程以提升速度。
7. 新增用于[数据表处理 Sharp 表达式](/pql/sharp-table.md) `TO TREE` 和 `COLLECT`。
8. 参数变量作用域设定为整个 PQL 过程。

## v1.4.0 (2021.4.30)

这个版本主要是 OneApi 相关的更新。

1. OneApi 现在支持[接口文档](/oneapi/document.md)和[流量统计](/oneapi/traffic.md)，但查阅页面暂时未实现。
2. OneApi 现在统一返回值的[数据格式](/oneapi/general.md)。
3. OneApi 新增设置`oneapi.ajax.settings`，设置[通用接口请求过滤器](/oneapi/setup.md)。
4. Voyager 已[支持缓存](/voyager/cache.md)。
5. Sharp 表达式新增表达式 ELSE、IF GREATER THAN ZERO、IF LESS THAN ZERO，详见 [Sharp 判断操作](/pql/sharp-if.md)。
6. 全局变量现在支持复合类型，如 TABLE、ROW 等。
7. Worker 现在所有参数名使用一个中线`-`开头，而不再是两个中线`-`。增加数据连接测试功能和相关参数。
8. 所有 test 相关文件已移走。

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
4. [Markdown 扩展语法](/voyager/markdown.md)新增行内间距，例如`-|` `20` `|-`。
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
13. [Marker 应用](/voyager/markdown.md)现在支持行间隔设置，如`--` `20` `--`表示在上下两行之间设置一个`20`像素的间隔。
14. `%`现在也是特殊字符，如遇到字符冲突时使用用`~u0025`代替，一般情况下不需要使用。
15. [Worker 执行器](/pql/worker.md)现在支持将参数写在`--file`参数指定的文件名后面。

## 以前的版本更新

v1.1.0 之前的版本更新可[点击这里](/pql/history.md)查看。

---
参考链接

* [PQL 概览](/pql/overview.md)
* [OneApi 统一接口](/oneapi/overview.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [DataHub 数据开发框架](/datahub/overview.md)

