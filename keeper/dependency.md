# 依赖 Dependency

在Keeper中，依赖分为前置依赖（紧前依赖）和后置依赖（紧前依赖）。前置依赖是调度任务运行的条件，前置条件必须满足时调度任务才会运行；后置依赖用于对计算结果的正确性进行检查，如果不正确则任务状态会变为“执行结果不正确”。依赖可以设置多个。

“无限循环任务”不支持设置前置依赖。其他类型的调度作业在手工“立即运行”时可以忽略前置依赖。

Keeper支持三种类型的依赖，分别为任务依赖、SQL依赖和PQL依赖。

## 任务依赖

**任务依赖** 是指当前调度作业的任务的创建后需要等待依赖的调度作业的任务执行完成才能启动。比如在数据仓库中，ODS层的任务完成后，DW层的任务才能启动，即DW层的任务启动依赖于ODS层任务完成。

* 任务依赖需要设定“任务时间表达式”，因为两个任务一般不会是同样的时间创建，这样就需要根据当前任务时间指定依赖的任务时间，即是依赖哪个时间的调度任务完成才执行当前任务。举例说明：当前任务时间为每天`03:00`，但是依赖的任务时间为每天`02:00`，那么依赖时间表达式就要设置成为`yyyy-MM-dd 02:00:00`，其中的`yyyy-MM-dd`部分在执行检查时会根据当前任务时间计算出来。也可以指定不是同一天的时间，如前一天的`23:30`，表达式就要写成`$task_time MINUS DAYS 1 FORMAT 'yyyy-MM-dd 23:30:00'`。这里是PQL中[Sharp表达式](/pql/sharp.md)的语法，详见[Sharp表达式中关于时间的操作](/pql/sharp-datetime.md)。
* 执行完成并不一定是“执行成功”，可以设置其他的结束状态，如执行失败、执行超时等。

调度作业的多个命令构成了调度作业的内部工作流，通过任务依赖，可以构建调度作业之间的工作流。在未来版本中，系统会提供作业工作流的管理功能。

注意，被依赖的调度作业不有轻易修改调度时间，否则会影响后续所有的依赖任务。另外，后置依赖不支持“任务依赖”设置。

## SQL依赖

**SQL依赖** 是日常生产过程另一种常用的依赖形式，依赖需要指定一个SELECT查询语句，系统会检查这个SELECT语句是否有返回结果，如果有结果表示依赖成立，否则会下一分钟继续检查。SQL依赖需要配置的项如下：

* **数据源名称**，即要在哪个数据库查询数据，需要预先[在qross.properties文件中设置好连接串](/pql/properties.md)。如果不设置，默认使用`jdbc.default`连接。
* **SELECT查询语句**，必填项，一定注意是查询有结果即依赖成立，所以要求`WHERE`条件一定得严谨。另外，SELECT语句查询的 **最后一行结果的值** 可以传递给任务的[命令](/keeper/dag.md)，占位符格式为`#{field}`。举例说明：查询语句为`SELECT name, age FROM students WHERE id=1`，DAG中的命令为`java -jar score.jar name=#{name}&age=#{age}`，命令中的`#{name}`和`#{age}`在依赖成功后会被替换成SELECT语句查询的值。DAG中任何类型的命令都支持这样传递值。
* **非查询语句**，选填项，在依赖成功后执行的可以在上面指定的数据库执行一条非查询语句，用于对数据库做一些额外的操作，如保存一条操作记录。这里同样可以使用SELECT语句的查询结果，传参规则同PQL中的[PUT语句](/pql/put.md)。例如：`UPDATE scores SET status='updating' WHERE name=&name AND age=#age`。

SQL依赖可用于后置依赖，即任务执行完成后的结果正确性检查，同样是如果查询有返回结果即判定任务的执行结果正确。

在SQL依赖的SELECT语句和非查询语句中，可以使用一些预设的变量。

* `$job_id` 依赖所属的调度ID
* `$task_id` 当前任务ID
* `$task_time` 当前任务时间, 格式是 “yyyy-MM-dd HH:mm:00”
* `$record_time` 当前任务的创建时间，格式是 “yyyy-MM-dd HH:mm:ss”

例如：`SELECT id FROM extra_tasks WHERE task_id=$task_id`。主要用于扩展Keeper。这些变量也可以通过参数的形式传递，如`#{task_id}`。

## PQL依赖

**PQL依赖** 可以理解为SQL依赖的高级版本，也可以理解为是自定义依赖。PQL依赖除了可以访问数据库以外，还可以进行[请求接口](/pql/request.md)、[读写文件](/pql/file.md)等操作。使用PQL依赖，可以检查某个接口的返回值是否正确、REDIS中的某个键是否有期望的值、文件是否存在等，详见[PQL相关的文档](/pql/overview.md)。PQL依赖的配置项如下：

* **数据源名称**，即要在哪个数据库查询数据，需要预先[在qross.properties文件中设置好连接串](/pql/properties.md)。如果不设置，默认使用`jdbc.default`连接。也可以在PQL过程中通过[OPEN语句](/pql/open.md)打开指定的数据源。
* **主PQL程序**，必须项。需要输入PQL过程语句并且需要有返回值，如何返回值请参阅[PQL中的OUTPUT语句](/pql/output.md)。依赖是否成立由PQL的返回值确定，成立规则如下：

    + TABLE/ROW/ARRAY等集合类型，不为空，即有数据
    + 字符串，可转换为`true`，如"true"、"yes"、"1"等
    + 布尔值：`true`
    + 数字：大于`0`
    + 其他类型：不为`null`

    SELECT语句返回值为TABLE，使用第一条集合规则。非查询语句返回影响的行数，使用数字规则。注意因为PQL的返回值不固定，所以并不能像SQL依赖一样向命令传递PQL的返回值。
* **额外PQL过程**，选填项，当依赖成功时额外执行的PQL过程，一般情况下用不到，因为所有的工作都可以在主PQL程序中完成。

在PQL依赖的主程序和额外程序中，可以使用一些预设的变量。

* `$job_id` 依赖所属的调度ID
* `$task_id` 当前任务ID
* `$task_time` 当前任务时间, 格式是 “yyyy-MM-dd HH:mm:00”
* `$record_time` 当前任务的创建时间，格式是“yyyy-MM-dd HH:mm:ss”
* `$retry_times` 当前依赖的重试次数

这些变量可以直接在PQL过程中使用。同样，这些变量也可以通过参数的形式传递，例如`#{job_id}`。
    
因为PQL依赖不能像SQL依赖一样传递查询结果给工作流中的命令，但是非要实现也有解决方案，需要使用Keeper的系统表，但不建议这么做，对系统表的更改可能会产生意外的结果。
```sql
GET # SELECT name, age FROM students WHERE id=1;
PUT # UPDATE scores SET status='updating' WHERE name=&name AND age=#age;

IF @COUNT > 0 THEN
    SAVE TO mysql.qross;
    PUT # UPDATE qross_tasks_dags SET command_text=REPLACE(command_text, '#{name}', &name) WHERE task_id=$task_id AND record_time=$record_time;
END IF;

RETURN @COUNT;
```

## 依赖的尝试次数

调度任务被创建后，有前置依赖的任务会 **每分钟** 检查一次依赖是否成立。对于“仅依赖触发”的调度作业，建议设置成为`0`，表示一直检查。其他类型的调度作业默认设置为`100`次，可根据实际应用场景增减。如果检查次数超过这个设置的最大值，调度任务状态会变为“检查超过限定次数”。

## 依赖相关事件

前置依赖和后置依赖分别相关两个异常事件。前置依赖如果检查次数超过最大限定次数，会触发“检查超过限定次数”事件；后置依赖如果检查结果不符合预期会触发“执行结果不正确”事件。可以在[事件和预警](/keeper/event.md)页面进行相应的设置。


---
参考链接

* [调度作业 Job](/keeper/job.md)