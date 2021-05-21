# 版本和更新

## v1.5.0 (开发中)

* [系统] 增加 Keeper [命令模板管理](/master/system/command-tempalte.md)。

## v1.4.0 (2021.5.4)

* [系统] 增加 PQL [全局变量](/master/system/variables.md)管理。

## v1.3.0 (2021.3.24)

* [系统] 增加 PQL [自定义系统函数](/master/system/functions.md)管理。

## v1.2.0 (2021.2.28)

这个版本主要是使用 Cogo 进行开发的尝试，更新了系统设置中的两个管理功能。

* [系统] 增加[数据源连接管理](/master/system/connections.md)功能。
* [系统] 增加[数据源配置管理](/master/system/properties.md)功能。
* [全局] root.css 从项目中分离。

## v1.1.0 (2021.1.31)

这个版本主要是 root.js 组件库的更新，这个项目基本没有更新。

* root.js 组件库从项目中分离。
* Home 页面任务统计逻辑修改为异步。
* 跟随 root.js 组件库更新修改多个页面。

## v0.6.5 (2020.12.25)

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

* **文档** 模块上线，第一版只有[PQL](/pql/overview.md)和[OneApi](/oneapi/overview.md)文档，其他文档不断补全中。 
* [调度] 调度详情页“自动开启时间”和“自动关闭时间”现在使用日历进行设置。
* [调度] 批量创建任务页面，“开始时间”和“关闭时间”现在可使用日历进行设置。
* [系统] 系统日志中新增“工作日历”，用于 [Cron 表达式](/keeper/cron.md)中的`W`和`R`关键字。
* [系统] 系统邮件发件人设置中移除`SSL`设置。


## v0.6.3

版本`0.6.3`的主要是事件相关的修改，更新内容如下：

* [调度] [事件](/master/job/events.md)现在不仅支持运行PQL过程，还可以运行Shell命令和Python脚本。Shell命令可支持多行。
* [调度] [自定义事件](/master/system/events.md)现在也支持通过Shell命令或Python脚本编写逻辑。也可接收传参和变量。
* [调度] Shell 命令的执行器升级，现在已经支持引号、分号和管道符等，而且现在可以写多行了。            
* [调度] [任务详情页](/master/jobs/task.md)的“中断任务”按钮，现在同时中断 **sh** 命令的子进程。
* [调度] [创建新的调度](/master/jobs/job.md)时不再添加默认事件。 
* [调度] [任务详情页](/master/jobs/task.md)瀑布流图增加状态颜色并修复问题。
* [调度] [DAG 页面](/master/jobs/dag.md)增加立即运行按钮，与[立即运行页面](/master/jobs/manual.md)的逻辑有一些不同。
* [全局] 新的[登录欢迎页](/master/home.md)，一眼掌握系统全局。
* [全局] 所有 **代码编辑器** 都更新了彩色编码，更方便编辑和阅读。
* [全局] [Qross 系统首页](/master/index.md)重做，现在可以自动创建或升级系统。
* [全局] 新增“关于 Qross”页面，可以了解Qross家族最新版本信息，找到源码链接或联系作者。
* [全局] Keeper 运行日志暂时去掉了分钟筛选，未来解决查询性能后恢复。

---
参考链接

* [Keeper 任务调度工具](/keeper/overview.md)
* [PQL 过程化查询语言](/pql/overview.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [root.js 组件库](/root.js/overview.md)