# 版本及更新

## 0.6.4
开发中...

## 0.6.3
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

* [Keeper中的DAG](/keeper/dag.md)
* [任务事件](/keeper/event.md)
* [自定义事件](/keeper/custom.md)
* [在Keeper中调度Shell命令](/keeper/shell.md)
* [Keeper API - Restful接口](/keeper/restful.md)
* [Keeper API - Message功能](/keeper/restful.md)
* [邮件模板](/keeper/template.md)
* [任务运行日志](/keeper/logs.md)