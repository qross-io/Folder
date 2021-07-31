# Keeper 监控

为了保证 Keeper 的正常运行，系统提供了一些保障和监控手段，在“系统”->“Keeper 监控”下可以看到。简要介绍如下。

## 节点管理

查看所有运行过的节点，请查阅 [Keeper 节点管理](/master/keeper/nodes.md)。

## 节点状态监控

查看某个节点的统计数据，请查阅 [Keeper 节点监控统计](/master/keeper/node.md)。

## 节点运行状态

查看节点的运行情况或重启节点，请查阅 [Keeper 心跳和重启](/master/keeper/beats.md)

## 运行日志

可以按小时查看 Keeper 的运行日志，这些日志保存在 Qross 系统家目录的`keeper`文件夹下。请查阅 [Keeper 运行日志](/master/keeper/running.md)

## 错误日志

运行日志中会记录全部的日志，错误日志只显示 Keeper 运行中的错误。请查阅 [Keeper 错误日志](/master/keeper/exceptions.md)

## 卡死的任务

调度任务有时会卡死，就是一直执行不完，也可能是执行完了进程没退出，虽然很少见但确实存在。一般情况下，Keeper 会自动恢复这些任务，但会将这些任务记录下来。

## Keeper 启停记录

Keeper 的每次启停都会记录下来，请查阅 [Keeper 启停记录](/master/keeper/starts.md)

## 工作日历

一般默认为周一到周五为工作日，周六周日为休息日，没有考虑法定节假日和企业本身的工作和休假制度。在工作日历可以设置特殊工作日和休假日，主要用于 [Cron 表达式](/keeper/cron.md)的工作日`W`和休息日`R`设置。


---
参考链接

* [Qross 系统安装](/qross/setup.md)
* [Keeper 设置](/keeper/settings.md)
* [Cron 表达式](/keeper/cron.md)

