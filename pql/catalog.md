# PQL文档目录

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
    + [PQL中极致简单的数据流转操作](/pql/dataflow.md)
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
    + [PQL中有返回值的语句](/pql/evaluate.md)

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
    + [在PQL中调用 Java 方法 INVOKE](/pql/invoke.md)
    + [在PQL中运行Shell命令 RUN](/pql/run.md)
    + [PQL中的 Javascript](/pql/javascript.md)    
    
* PQL相关应用
    + [统一接口 OneApi](/oneapi/overview.md)
    + [模板引擎 Voyager](/voyager/overview.md)
    + [任务调度工具 Keeper](/keeper/overview.md)
    + [数据管理平台 Master](/master/overview.md)
    + [PQL执行器 Worker](/worker/overview.md)

* 附录
    + [完整的嵌入规则表](/pql/place.md)
    + [特殊字符表](/pql/characters.md)
    + [io.qross.pql.PQL 类](/pql/class.md)
    + [PQL全局设置](/pql/setup.md)
    + [PQL执行器 - Worker](/pql/worker.md)