# Keeper 异常日志

运行日志中会记录全部的日志，异常日志只显示 Keeper 运行中的错误。这些错误可能是 Keeper 本身的 Bug，也可能是设置问题，这些错误可以帮助使用者或作者快速定位问题。

在管理页面上只显示最近 12 天（次）的错误记录，超过 12 天的日志仍可以在数据库中查看，对应数据表`qross_keeper_exceptions`，这个表中只记录异常日志。

如果已经在[系统设置](/master/system/settings.md)中配置了发件邮箱等相关信息，则可以通过页面上方中间的蓝色按钮“将错误发送给作者”把异常信息发送到作者邮箱<wu@qross.io>，作者会提供免费的技术支持服务。

---
参考链接

* [Keeper 运行日志](/master/keeper/running.md)
* [Qross 系统设置](/master/system/settings.md)