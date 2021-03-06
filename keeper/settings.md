# Keeper 系统设置

在“系统”->“Keeper 监控”->“Keeper 设置”中可以看到 Keeper 相关的所有设置项。简单说明如下：

* 单 CPU 任务并行度。指一个 CPU 核心最多可以同时运行几个命令。任务调度程序是一个主要使用 CPU 资源的程序，对内存的使用反而很小。如果 CPU 的线程数为`16`，单 CPU 并行度设置为`4`，那么 Keeper 最多可同时运行`64`个命令。实际生产中建议根据任务对资源的消耗情况合理调整这个值的大小，一般建议设置为`4`或`8`。

* 心跳邮件发送频率。系统会发心跳提醒邮件给管理员，以确定 Keeper 是否运行正常。这个频率可根据业务需要进行设置。在 Keeper 异常时会即时发送邮件，不会定时发送。

* Keeper 接口服务的 IP 地址和端口。Keeper 提供了一系列 [Restful 接口](/keeper/rest.md)用于和 Keeper 交互。当前版本 Keeper 需要和 Master 管理程序运行在同一台机器上，IP 设置为`localhost`或本机地址即可。端口默认为`7700`。

* 系统日志保留天数。除了任务运行日志以外，Keeper 运行还会生成一些的系统日志，这些日志长期积累会占用磁盘空间，在调度作业比较多时（比如大于500）这个空间还是会比较大的。且日志具有时效性，如果这个参数不设置为`0`（表示永久保留），那么系统会自动清理超期的系统日志，以节省磁盘空间。

* 发送错误日志给作者。这个开关开启时，假如 Keeper 在运行过程中发生了错误，系统会自动发送错误邮件到作者的邮件，以帮助作者定位问题以改进系统。在“错误日志”管理页面中，也有主动发送功能。

---
参考链接

* [Restful 接口服务](/keeper/rest.md)
* [Keeper 监控](/keeper/monitor.md)