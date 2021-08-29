# 版本及更新

## v1.8.0 (2021.8.30)

* 修改几个 Bug。

## v1.7.0 (2021.7.29)

* 按调度工作流重启任务已支持。
* 按项目分组或批量重启任务已支持。
* Restful 接口已支持 Token 认证，Token 需要预先设置，对应全局变量`@KEEPER_HTTP_TOKEN`。
* 统计计数工作改为即时统计，按小时和按天统计逻辑移除。
* 大量的优化工作：
    + 运行中任务从主任务表备份到新表。
    + 未检查逻辑从主任务表移出。
    + 异常任务单独保存到新表。
    + 待重启任务单独保存到新表。
    + 全任务相关表索引优化。

## v1.6.0 (2021.6.30)

这个版本改动巨大，共 84 项更新。为了实现多机部署，基本所有的逻辑都涉及到改动。

* [对等集群模式](/keeper/cluster.md) 现以支持，允许在一个局域网不同的机器上启动 Keeper，实现高可用和分散式计算。
* [Keeper 节点事件](/master/keeper/node-events.md)加入，可以对 Keeper 生命周期的任何阶段进行编程了。
* 新的任务状态 `waiting_limit`，表示在任务准备状态时，因为并发数限制不能执行，等待超过预定时间。
* 3 个系统任务移除，整合进核心逻辑。
* 一些 Bug 修复。

## v1.5.0 (2021.5.28)

已 4 个版本没有更新的 Keeper 了，这个版本带来了全新功能。

* Keeper 现已支持[命令模板](/keeper/command-template.md)，以减轻工作流中重复命令和脚本的设置工作。也可以作为数据专用场景服务，如数据集成、数据质量检查等。
* Shell 命令和 Python 脚本中的参数支持由基本规则嵌入改为 [PQL 嵌入规则](/pql/embedded.md)，以实现更复杂的逻辑和功能。改变涉及工作流、命令模板、执行命令和脚本事件和自定义事件。这项改动不兼容旧版本。
* [调度专有变量](/keeper/job-variable.md)已支持，作用域为单个调度作业的所有任务实例。
* 脚本和命令的参数恢复支持，已为命令提供更多的定制功能。
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