# Keeper 调度变量

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

SET %update_time := @BUFFER LAST ROW -> GET 'update_time';
```

假设`oracle.primary`是业务主库，`mysql.warehouse`是分析从库，以上逻辑实现了主库到从库的表`students`的数据增量抽取。其中`%update_time`即调度变量，逻辑中每次只查询大于`%update_time`的数据，在数据迁移完成后更新`%update_time`的值为源数据表中的最后更新时间。PREP 语句用于预防迁移过程中可能出现错误或失败，则在迁移前删除已更新的数据以防止数据重复。`%update_time`的作用域仅限于这个 ID 为`5`的调度作业（Job）的各个任务实例（Task）中，但其他调度作业的任务不能使用这个变量。

调度变量的在 PQL 中使用时以`%`开头，以区分于用户变量的`$`和全局变量的`@`。因为可以在 Shell 和 Python 脚本中使用[嵌入式 PQL](/pql/embedded.md) 的语法书写 PQL，所以这两类脚本中也可以使用调度变量，但其逻辑和生命周期只能在执行 Shell 或 Python 运行之前。

```shell
<% 
IF %COUNTER IS UNDEFINED THEN
    SET %COUNTER := 0;
END IF;

IF %COUNTER % 2 == 0 THEN
%>
java -jar /usr/local/test.jar <%= $job_id %> -counter <%= %COUNTER %>
<% ELSE %>
python3 /usr/local/test.py <%= $task_time FROMAT 'yyyyMMdd' %>
<% END IF;

SET %COUNTER := %COUNTER + 1;
%>
```

上例的逻辑简单可以说明为：在某个调度的工作流中的一个命令中设置了一个调度变量`%COUNTER`用来计数。当`%COUNTER`为单数时，执行 Python 命令; 当`%COUNTER`为复数时，执行 Java 命令。在实际运行之前，会先解析 PQL 嵌入语句，然后生成最终要执行的 Shell 语句。

调度变量可以在工作流中通过`SET`或`VAR`语句进行定义和赋值，也可以通过 Master 管理工具的调度页面中[调度变量](/master/job/variables.md)进行管理，比如查看当前调度已经定义的变量，并且手工对调度变量的值进行修改。

---
参考链接

* [Keeper 工作流](/keeper/dag.md)
* [PQL 用户变量](/pql/variable.md)
* [PQL 全局变量](/pql/global-variable.md)