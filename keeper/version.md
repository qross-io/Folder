# 版本及更新

## v1.5.0

已 4 个版本没有更新的 Keeper 了，这个版本带来了全新功能。

* Keeper 现已支持[命令模板](/keeper/command-template.md)，以减轻工作流中重复命令和脚本的设置工作。
* Shell 命令和 Python 脚本中的参数支持由基本规则嵌入改为 [PQL 嵌入规则](/pql/embedded.md)，以实现更复杂的逻辑和功能。改变涉及工作流、命令模板、执行命令和脚本事件和自定义事件。这项改变不兼容旧版本。
* [调度专有变量](/keeper/job-variable.md)已支持，作用域为单个调度作业的所有任务实例。
* 命令参数恢复支持，已为命令提供更多的定制功能。
* 配合 Master 增加一些 [Restful 接口](/keeper/rest.md)。

## v1.4.0

* 配合 Master 增加一些 [Restful 接口](/keeper/rest.md)。

## v1.3.0

* 配合 Master 增加一些 [Restful 接口](/keeper/rest.md)。

## 以前的版本更新

v1.3.0 之前的版本更新可[点击这里](/keeper/history.md)查看。

---
参考链接

* [Cron表达式](/keeper/cron.md)
* [Keeper中的DAG](/keeper/dag.md)
* [任务事件](/keeper/event.md)
* [自定义事件](/keeper/custom-event.md)
* [Keeper API - Restful接口](/keeper/restful.md)