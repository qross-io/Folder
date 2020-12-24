# 事件 Event

事件主要对调度任务Task进行扩展，在任务达到其生命周期的某一状态时，可以触发相应的事件。通过这些事件，可以对调度任务进行扩展，实现特定的功能，如提醒或预警等。不同的状态触发的事件可能不同，另外系统提供了[自定义事件](/keeper/custom-event.md)的方法，以实现更多个性化的事件，如电话预警、企业微信提醒等。

## 可触发事件的状态

不是所有状态都会触发事件，可触发事件的状态如下：

* 任务新创建时。
* 任务依赖失败或检查次数超过上限时，这个次数可以在设定依赖时设置，默认值为`100`次（每分钟`1`次），详见[依赖设置](/keeper/dependency.md)。
* 任务准备好（依赖成功）时，仅设置了依赖的任务可以触发这个事件。
* 任务执行失败时，即任务执行出错时。这个状态的事件非常常用。
* 任务执行结果不正确时，执行完成后可以通过后置依赖对结果进行检查，详见[依赖设置](/keeper/dependency.md)。
* 任务执行超时时，在添加命令时可以设置这个命令的执行超时时间，详见[工作流设置](/keeper/dag.md)。
* 任务执行成功时。
* 任务运行缓慢时，可以设置任务的整体运行时间，超过这个时间则会触发这个事件，详见[调度作业](/keeper/job.md)中的说明。

对于中间状态或正常状态，任务新建、依赖成功和执行成功，非常重要的调度作业可以设置这个状态的事件，一般不需要。其他异常状态经常需要设置事件，多用于预警。

## 系统预设的事件

这些事件实现了调度中常用的功能。

* 发送邮件。最基本的预警和提醒功能，可以选择收件人，请参阅[Qross系统角色说明](/qross/role.md)。这个功能需要在系统中设置好发件人。
* 请求接口。需要指定接口地址，可以选择请示接口的Method。如果多个调度作业都用到同样的接口，可以设置为[自定义事件](/keeper/custom-event.md)。在接口地址中，可以使用一些内置的参数或变量，如`#{job_id}`或`$job_id`，也可使用[Sharp表达式](/pql/sharp.md)对变量进行编辑，如`${ $task_time FORMAT 'yyyy-MM-dd' }`。简单说明如下：
    + `job_id` 调度作业的ID
    + `task_id` 任务ID
    + `task_time` 任务创建时间，格式`yyyyMMddHHmm00`
    + `record_time` 任务记录时间，格式`yyyy-MM-dd HH:mm:Ss`
    + `title` 调度作业的标题
    + `event_limit` 事件触发限定的值，多个值由逗号分隔
* 执行脚本。这里同工作流中的命令一样，支持Shell、PQL和Python。同样，如果多个调度使用同样的脚本，可以设置为[自定义事件](/keeper/custom-event.md)。
* 重启任务。在异常状态下可以延时自动重启任务，可以设置延时时间和重启次数上限。注意这个和命令中的自动重试不同，这个的触发在自动重试之后且可以有延时。由于程序运行依赖的外部环境问题而引起的程序出错可设置这个事件，比如数据库当机时间较长时。

## 事件触发限定

在Keeper中，调度任务启动有`4`种方式。

* 自动启动，值`auto_start`，指系统自动调度创建并启动的任务。
* 手工启动，值`manual_start`，指在“立即运行”页面或“命令列表”页面手工运行的任务，通常这种情况不需要触发事件。
* 自动重启，值`auto_restart`，指系统由于任务运行异常的事件发生的自动重启。
* 手工重启，值`manual_restart`，指在“任务详情页”使用“重新启动”按钮重新启动的任务，通常这种情况下不需要触发事件。

可以为每一个事件选择在以上`4`种情况下是否触发事件。

## 自定义事件

在系统中自定义的事件都会显示在“事件”页面中，详情参阅[自定义事件](/keeper/custom-event.md)。


---
参考链接

* [调度作业 Job](/keeper/job.md)
* [自定义事件](/keeper/custom-event.md)