# 版本和更新

**Qross Master** 相对于版本`0.6.2`，版本`0.6.3`的主要更新内容如下：
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
2. 所有 **代码编辑器都** 更新了彩色编码，更方便编辑和阅读。
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