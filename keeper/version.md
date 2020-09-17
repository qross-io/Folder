# 版本及更新

## v0.6.4
这个版本主要是对[Cron表达式](/keeper/cron.md)进行了升级和扩展。

1. [Cron表达式](/keeper/cron.md)现在更加强大，已经可以满足所任何时间的调度计划。
    * 现在可以自定义工作日和休息日，不再只把周一到周五当工作日，周六周日当休息日。在Master中“系统”->“Keeper监控”->“工作日历”中进行设置。
    * 现在`天`设置支持关键词`F`、`R`、`LW`、`LR`、`FW`、`FR`，分别表示第一天、休息日、最后一个工作日、最后一个休息日、第一个工作日，第一个休息日
    * 现在`周`设置支持关键词`F`、`L`、`W`、`R`、`LW`、`LR`、`FW`、`FR`，分别表示当月第一周、当月最后一周、工作日、休息日、当周最后一个工作日、当周最后一个休息日、当周第一个工作日、当周第一个休息日。如`FW#2`表示第二周的第一个工作日。
    * `天`设置现在支持`1-24/2`这样的设置，表示`1`到`24`号每隔`2`天执行一次。
    * `周`设置现在支持整周的设置，如`#F`、`#2`、`#L`，分别表示第一周、第二周、最后一周，最多支持到`#6`。
    * `周`设置在指定第几周时同时支持混合设置，如`MON-WED+FRI#3`，表示第三周的周一到周三和周五，注意是加号不是逗号。
    * `周`设置现在支持指定当月的第几个周几，如`FMON`、`LSUN`、`2THU`，分别表示第一个周一、最后一个周日和第二个周四，注意与第几周的周几意义不同。
    * 易读格式增加大量的关键词并完善每周和每月的设置，如`WEEKLY WORK-DAY 12:00`、`MONTHLY LAST-WEEK 03:00`，分别表示每周的工作日`12`点和每月最后一周每天凌晨`3`点。
    * [不兼容] 周一到周日的数字顺序由`2,3,4,5,6,7,1`修改为`1~7`，更符合日常使用习惯。
    * [不兼容] `周`设置不再支持符号`/`，可使用逗号枚举或加号连接多`天`。
2. 增加一个系统调度任务，自动清理任务日志和运行记录，可在“Keeper设置”中设置保留多少天的日志或记录。
3. [优化] `Monitor`逻辑已从`Actor`中移除，已减少线程占用。
4. 其他的一些非功能更新。

## v0.6.3
当前 **Keeper** 稳定版本为 0.6.3-RELEASE，这个版本主要是Shell命令和事件相关的升级。

1. [DAG](/keeper/dag.md) 中的Shell命令优化。现在已支持双引号、管道符和分号，多行Shell命令也已支持！
2. 在[事件](/keeper/event.md)中可以执行 **Python** 和 **Shell** 脚本了。
3. [自定义事件](/keeper/custom.md)现在同样可以用 **Python** 或 **Shell** 进行定义。
4. 中断正在运行的[Shell命令](/keeper/shell.md)时（仅限`.sh`脚本），同时会杀死子进程。
5. [Restful接口](/keeper/restful.md)新增加`task/instant/{jobId}`，用于即时创建任务，适于用业务流程触发生成任务。
6. 因为任务失败事件[邮件模板](/keeper/template.md)导致任务失败时邮件发不出去的问题已修复。
7. [**任务日志**](/keeper/logs.md)中的乱码问题已解决。

不再兼容旧版本的更新

1. Shell命令中的全局变量 %JAVA_BIN_HOME 不再支持，直接去掉即可。
2. 在实时生成任务的[Restful接口](/keeper/restful.md)和[Message功能](/keeper/message.md)中，参数占位符由`${param}`变为`#{param}`。


---
参考链接

* [Cron表达式](/keeper/cron.md)
* [Keeper中的DAG](/keeper/dag.md)
* [任务事件](/keeper/event.md)
* [自定义事件](/keeper/custom.md)
* [在Keeper中调度Shell命令](/keeper/shell.md)
* [Keeper API - Restful接口](/keeper/restful.md)
* [Keeper API - Message功能](/keeper/restful.md)
* [邮件模板](/keeper/template.md)
* [任务运行日志](/keeper/logs.md)