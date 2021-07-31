# Keeper 节点启动记录

这个页面会记录当前节点所有的启动记录。一般情况下，关机和重启分为以下几种情况：

* 在[心跳和重启](/master/keeper/beats.md)功能页面进行的关机，在关机后由 crontab 自动重启或手工重启。这种情况属于正常重启，会记录停机时间和持续时间。
* 在操作系统中杀掉 Keeper 进程，再由 crontab 自动重启或手工重启。这种情况属于异常启动，不会记录停机时间和持续时间。
* Keeper 本身出错或因为操作系统资源等原因自动退出，再由 crontab 自动重启或手工重启。这种情况也属于异常启动，不会记录停机时间和持续时间。

在 Linux 系统中，强烈建议[配置 crontab](/qross/install.md)，配置好的 crontab 会自动检查 Keeper 是否还存在，如果检查不到进程则会自动启动 Keeper。所以，crontab 是实现 Keeper 自动重启的必要设置。

---
参考链接

* [Keeper 节点心跳和重启](/master/keeper/beats.md)
* [Qross 系统安装](/qross/install.md)