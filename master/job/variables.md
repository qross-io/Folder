# 管理调度专有变量

调度专有变量（Job Context Variable）是 PQL 为 Keeper 做得扩展，调度变量的作用域是这个调度的工作流中的所有命令和脚本，即在所有任务运行实例（Task）中共享。调度变量常用于数据的增量更新，在调度变量中保存增量字段的值，可以在下次调度任务运行时使用最新的值。在 [Keeper 命令模板](/keeper/command-template.md)中，调度变量应用较多。更多信息请参考 [Keeper 调度变量](/keeper/job-variable.md)。

调度变量本身是 PQL 的内容，所以在 Keeper 中，可以在任何可以使用 PQL 的地方使用调度变量。包括工作流的命令和脚本，SQL 依赖和 PQL 依赖，事件触发的接口或要执行的的逻辑等等，会在相应的章节进行说明。

调度变量的基本属性如下：

* 变量名，即以百分号`%`开头的部分，如`%UPDATE_TIME`。
* 变量描述，方便对值的具体信息进行了解，需要手工更新和维护，只用于管理目的。
* 变量类型，即 PQL 支持的数据类型，如`TEXT`、`INTEGER`等。特别的，有一个`SQL`类型，其本质是是一个字符串，只是为了方便编辑和查看。
* 变量的值，可以是 PQL 支持的任意类型。

可以在管理页面手工创建、更新和删除调度变量，更新后会影响以后的任务实例（Task），当然也可以在 Keeper 工作流中定义和更新调度变量，例如：

```sql
IF %TEST IS UNDEFINED THEN
    SET %TEST := 'Hello World';
END IF;
```

使用`UNDEFINED`判断变量是否已经定义，删除已有的变量会让这个变量重置为`UNDEFINED`状态。

---
参考链接

* [Keeper 调度变量](/keeper/job-variable.md)
* [PQL 全局变量](/pql/global-variable.md)
* [PQL 用户变量](/pql/variable.md)