# 升级 Qross 系统

一般情况 Qross 的升级过程如下：

1. 将所有相关文件都上传到 Qross 系统家目录下。一般包括下面的文件：
    + `qross-keeper-x.x.x.jar`
    + `qross-master-x.x.x.jar`
    + `qross-worker-x.x.x.jar`
2. 修改`qross-keeper-start.sh`文件中的版本号为最新版本。
3. 关闭 Master 程序，启动新的 Master 程序。
4. 打开首页如 "http://localhost:8080/"，注意不是登录页"http://localhost:8080/login"，点击按钮进行升级。
5. 在"系统">"Keeper 监控">"心跳和重启"里重启 Keeper。
6. 完成升级

---
参考链接

* [安装 Qross 系统](/qross/install.md)