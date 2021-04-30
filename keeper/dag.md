# 工作流 DAG

工作流是任务调度的主要内容，是调度作业的核心，由一个或多个命令、程序或脚本构成，Keeper 把这些命令、程序或脚本统一称为`Command`。多个`Command`之间可以存在上下游关系，这种上下游关系构成 **DAG**，即“有向无环图”。下游`Command`可以依赖一个或多个上游`Command`，多个下游`Command`可以依赖同一个上游`Command`。

## 命令 Command

在当前的 Keeper 系统中，单个`Command`可以是 Shell 命令、PQL 过程或 Python 脚本。

### Shell 命令

Shell 命令基本上已经可以满足几乎所有的配置要求，因为所有的程序都可以转成一行 Shell 命令的形式，PQL 过程和 Python 脚本都可以配置成 Shell 命令。这里还 **支持多行Shell命令**，在运行时会自动保存为`sh`文件并添加可执行权限，注意这个功能仅在 Linux 系统中支持且启动 Keeper 程序的用户有相应的权限。

很多时候，我们需要为`Command`传递一些变化的参数，特别是日期时间。举个例子，如程序运行时间是凌晨 1 点，但希望计算前一天的数据，虽然可以在计算程序中写死，根据当前日期得到前一天的日期，但是这样的话这个程序就具体时效限制，即必须在当天执行成功才可以，如果追溯数据或失败重跑则只能计算前一天的数据。这种情况下，通常会把要计算的日期通过参数传递给`Command`。通过 PQL 的嵌入式功能，我们可以很方便的向`Command`传递参数。

```sh
java -jar /test/hello.jar ${ $task_time MINUS DAYS 1 FORMAT 'yyyyMMdd' }
```

上例中，Keeper 在运行这个命令时会自动计算`${ $task_time MINUS DAYS 1 FORMAT 'yyyyMMdd' }`的值然后将结果作为 Shell 命令的一部分执行。PQL 支持的嵌入内容都可以在这里使用，请参阅 [PQL 嵌入规则表](/pql/place.md)及各部分的相关内容。这里最常用的是[变量](/pql/var.md)、[函数](/pql/function.md)和 [Sharp 表达式](/pql/sharp.md)，可以使用所有的全局变量和以下已赋值的变量：

* `$job_id` 调度作业的唯一 id。
* `$job_type` 调度类型，枚举值为`scheduled`、`endless`、`dependent`和`manual`，分别对应时间计划任务、无限循环任务、依赖触发任务和手工执行的任务。
* `$task_id` 任务副本的唯一 id。
* `$command_id` Command 的唯一 id。
* `$action_id` Command 的实例 Action 的唯一id。
* `$task_time` 任务副本的时间，秒位为`0`，重要且常用。
* `$record_time` 任务记录的时间，和`$task_time`有区别，详见[调度任务](/keeper/task.md)中的说明。
* `$command_type` Command 类型，检举值为`shell`、`pql`和`python`，分别代表 Shell 命令、PQL 过程和 Python 脚本。
* `$retry_limit` Command 出错时的重试次数上限，下面有说明。
* `$overtime` Command 的超时时间限制，下面有说明。

只有 Shell 命令支持以上形式的变量和变量再计算，另外，所有类型的`Command`都支持与`OneApi`和`Voyager`相同的传参形式。如

```sh
java -jar /test/hello.jar ${ '#{task_time}' MINUS DAYS 1 FORMAT 'yyyyMMdd' }
```

或

```sql
PRINT '#{task_time}';
```

可用的参数同上面列出的预定义变量，但注意这些参数都没有类型，时间和字符串类型不会自动加引号。

### PQL 过程

PQL 过程也可以在`Command`中直接使用，在执行时 Keeper 会调用 [Worker 执行器](/pql/worker.md)来执行这个 PQL 过程。PQL 过程也可以保存为文件，通过 Shell 命令执行，如`PQL --file /usr/qross/pql/test.sql`。`PQL`命令可用的参数详见 [Worker 执行器](/pql/worker.md)。同样，PQL 过程也支持`#{arg}`格式的参数，可用参数见前面的说明。

### Python脚本

Python 脚本同样可以在`Command`中直接使用，在执行时会自动保存为`py`文件并执行这个文件，默认为`Python3`的执行器，注意运行机器上必须安装`Python3`。Python 脚本也支持`#{arg}`格式的参数，可用参数见前面的说明。

### 自定义执行器

自定义执行器已在开发计划中，将在未来的版本中支持。

## 命令的属性

除了命令的类型和内容外，还包含其他几个属性。

### 命令的名称

虽然命令的名称可以由任何字符构成，但是仍建议在设定的名称清晰易读，调度首页的搜索同时会搜索这个名称。

### 超时时间

设定一个时间，当命令的执行时间大于这个设置时间时，Keeper 会杀掉这个命令并重新运行，在达到最大次数时会触发 **任务执行超时** 事件，可以在事件中指定相应的处理方式。这个属性的单位的秒，默认值为`0`，表示不限制时间。这个属性建议慎重使用，因为程序多次运行并不能保证结果的一致性，除非你的计算程序中做了处理。

### 最大重试次数

在命令运行超时或失败时，自动重新运行这个命令，这个次数表示最大的重试次数。一般设定为`0`到`3`次。由于外部环境而不是程序本身引起的错误，比如数据库短时间连接失败等，可设置这个参数来避免。

### 上下游关系

这个设置非常重要！在命令列表页可以指定命令的上游 Command 的 ID 来设置命令的上下游关系，格式必须为`(id)`，如`(2)`表示当ID为`2`的命令执行成功后才执行当前的命令，`(23)(24)`表示当 ID 为`23`和`24`的命令执行完成后才执行当前的命令。在未来的版本中，系统将提供图形化的方式配置 DAG。


---
参考链接

* [调度作业 Job](/keeper/job.md)