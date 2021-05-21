# Keeper 中的名词术语

由于调度工具的特殊性，Keeper 有自己的一些专业术语。这些术语基本与行业通用，有的可能是专有的，但也很好理解。

## 调度作业 Job

**Job** 是调度计划的基本单位，实现某一计算功能的程序。可以由任何语言编写，并可以在服务器上运行。作业包含很多信息，比如名称、描述、运行时间配置、所有者等。

## 调度任务 Task

**Task** 是 Job 的实例，是 Job 的每一次执行。Task 包含的描述信息包括，任务时间，启动时间，完成时间，执行状态等。为了表达更加清楚，调度和任务在文档中经常会以英文单词 Job 和 Task 进行表述。

## 项目 Project

项目是 Job 的组织方式，仅用于管理 Job，是 Job 的分组。

## [所有者 Owner](/keeper/owner.md)

表示 Job 的开发者或负责人，也可以理解为预警信息的接收者。

## [工作流 DAG（有向无环图）](/keeper/dag.md)

每一个调度可以包含不仅一个程序或脚本，程序和脚本之间可以设定依赖关系，这种多个任务之间先后执行的依赖关系称为 **DAG**。

## 命令 Command

DAG 中的每一个脚本或命令称为 **Command**，Command 支持任何 Shell 命令、PQL 过程或 Python 脚本。一个或多个 Command 构成整个 DAG。

## 动作或执行 Action

Command 的每一次执行称为 **Action**，即 Action 是 Command 的实例。

## [依赖 Dependency](/keeper/dependency.md)

即任务执行的前置条件。除了时间条件外，还可以设置其他的依赖条件，如某个 Job 执行完、某个数据表内有特定的数据、某个特定的文件存在等。

## 任务状态 Status

任务 Task 从创建到运行完成的整个生命周期包含非常多的状态，正常状态有新建`New`、 检查依赖中`Initialized`、准备执行`Ready`、执行中`Executing`、执行完成`Finished`、执行成功`Success`，异常状态有检查超过最大次数`CheckingLimit`、失败`Failed`、结果不正确`Incorrect`、超时`Timeout`、被中断`Interrupted`。不同的任务状态会有不同的颜色表示。任务状态对日常管理非常重要。

## [事件 Event](/keeper/event.md)

任务 Task 在不同的状态可以触发不同的动作，这些动作可以执行不同的功能，比如发送邮件、重启任务、执行特定脚本等，这些预设的动作称为 **事件**。事件可以方便对任务的生命周期进行控制，特别是任务出错时的预警。

## [Cron 表达式](/keeper/cron.md)

**Cron表达式** 是行业内对调度任务时间计划设定的统一标准，Keeper 对 Cron 表达式进行了扩展，几乎可以满足任何指定时间的设定要求。标准 Cron 表达式有 7 位，在 Keeper 中，秒位在计划作业中无意义，必须保持为`0`。请参阅 [Cron 表达式文档](/keeper/cron.md)。


## 心跳 Beat

因为 Keeper 程序必须一直处于运行状态，在运行过程中，Keeper 需要每隔 **一分钟** 进行一次整体的检查，比如检查有没有任务需要执行、检查依赖是否满足等，如果有，则运行或检查任务。心跳是 Keeper 正常运行的标志，心跳在每分钟的`0`秒进行。

---
参考链接

* [调度 Job](/keeper/job.md)
* [任务 Task](/keeper/task.md)
* [工作流 DAG](/keeper/dag.md)
* [依赖 Dependency](/keeper/dependency.md)
* [事件 Event](/keeper/event.md)
* [Cron 表达式](/keeper/cron.md)