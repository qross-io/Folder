# 版本和更新

## v0.6.5

预计开发的“接口”模块未能进行下去，待 Cogo 完成后开发。这个版本主要是Bug修复和易用性更新。

* Master 安装过程更新，修改一些 Bug。
* 没有权限不再能更新 DAG 的内容。
* 依赖增加彩色编码。
* 仅时间计划调度支持修改“下一次运行时间”。
* 仅时间计划调度和无限循环调度显示 Cron 表达式。
* “下一次运行时间”现在可以通过日期时间选择进行修改。
* 其他的多个 Bug 修复。


## v0.6.4

这个版本主要是文档和Cron表达式相关的修改。

### 文档
**文档** 模块上线，第一版只有[PQL](/pql/overview.md)和[OneApi](/oneapi/overview.md)文档，其他文档不断补全中。 

### 调度
* 调度详情页“自动开启时间”和“自动关闭时间”现在使用日历进行设置。
* 批量创建任务页面，“开始时间”和“关闭时间”现在可使用日历进行设置。

### 设置
* 系统日志中新增“工作日历”，用于[Cron表达式](/keeper/cron.md)中的`W`和`R`关键字。
* 系统邮件发件人设置中移除`SSL`设置。


## v0.6.3

版本`0.6.3`的主要是事件相关的修改，更新内容如下：

### 调度
1. [事件](/master/job/events.md)现在不仅支持运行PQL过程，还可以运行Shell命令和Python脚本。Shell命令可支持多行。
2. [自定义事件](/master/system/events.md)现在也支持通过Shell命令或Python脚本编写逻辑。也可接收传参和变量。
3. Shell命令的执行器升级，现在已经支持引号、分号和管道符等，而且现在可以写多行了。            
4. [任务详情页](/master/jobs/task.md)的“中断任务”按钮，现在同时中断 **sh** 命令的子进程。
5. [创建新的调度](/master/jobs/job.md)时不再添加默认事件。 
6. [任务详情页](/master/jobs/task.md)瀑布流图增加状态颜色并修复问题。
7. [DAG页面](/master/jobs/dag.md)增加立即运行按钮，与[立即运行页面](/master/jobs/manual.md)的逻辑有一些不同。

### 全局
1. 新的[登录欢迎页](/master/home.md)，一眼掌握系统全局。
2. 所有 **代码编辑器** 都更新了彩色编码，更方便编辑和阅读。
3. [Qross系统首页](/master/index.md)重做，现在可以自动创建或升级系统。
4. 新增“关于Qross”页面，可以了解Qross家族最新版本信息，找到源码链接或联系作者。
5. Keeper运行日志暂时去掉了分钟筛选，未来解决查询性能后恢复。

---
参考链接

* [登录欢迎页](/master/home.md)
* [创建和升级Qross系统](/master/index.md)
* [任务的生命周期控制-事件](/master/job/events.md)
* [扩展Keeper-自定义事件](/master/system/events.md)
* [查看任务的执行情况](/master/jobs/task.md)
* [创建新的调度](/master/jobs/job.md)
* [管理调度的DAG](/master/jobs/dag.md)