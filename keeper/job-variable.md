# 调度变量

调度变量是 PQL 专门为 Keeper 做的扩展，与[用户变量](/pql/variable.md)和[全局变量](/pql/global-variable.md)的区别主要是作用域，用户变量作用最大范围是当前 PQL 过程，全局变量的作用域是任何 PQL 过程，调度变量的作用域仅限于当前调度作业（Job），可以在不同的任务实例（Task）之间共享这个变量。

假设某个调度作业的 ID 为`5`，其 DAG 中某个命令的逻辑如下：

```sql
OPEN oracle.primary;
IF %update_time IS UNDEFINED THEN
    GET # SELECT name, age, score, update_time FROM students LIMIT 10000;
ELSE
    GET # SELECT name, age, score, update_time FROM students WHERE update_time>%update_time LIMIT 10000;
END IF;
SAVE TO mysql.warehouse;
    PREP # DELETE FROM students WHERE update_time>%update_time;
    PUT # INSERT INTO students (name, age, score);

SET %update_time := @BUFFER -> LAST ROW -> GET 'update_time';
```

假设`oracle.primary`是业务主库，`mysql.warehouse`是分析从库，以上逻辑实现了主库到从库的表`students`的数据增量抽取。其中`%update_time`即调度变量，逻辑中每次只查询大于`%update_time`的数据，在数据迁移完成后更新`%update_time`的值为源数据表中的最后更新时间。PREP 语句用于预防迁移过程中可能出现错误或失败，则在迁移前删除已更新的数据以防止数据重复。`%update_time`的作用域仅限于这个 ID 为`5`的调度作业（Job）的各个任务实例（Task）中，但其他调度作业的任务不能使用这个变量。

调度变量的在 PQL 中使用时以`%`开头，以区分于用户变量的`$`和全局变量的`@`。因为可以在 Shell 和 Python 脚本中使用[嵌入式 PQL](/pql/embedded.md) 的语法书写 PQL，所以这两类脚本中也可以使用调度变量，但其逻辑和生命周期只能在执行 Shell 或 Python 运行之前。

---
参考链接

* [Keeper 工作流](/keeper/dag.md)
* [PQL 用户变量](/pql/variable.md)
* [PQL 全局变量](/pql/global-variable.md)