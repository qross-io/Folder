* 将所有文件都放在 QROSS_HOME 目录下
1. data文件夹下的calendar.csv文件是日历数据
2. pql目录下的两个文件是系统任务脚本
3. 修改qross-keeper-start.sh文件里的版本号为 0.6.4
4. worker, master, keeper 文件都分别上传
5. 关闭服务器上的master，启用新master
6. 打开首页如 "http://localhost:8080/"，注意不是登录页"http://localhost:8080/login"，点击按钮进行升级。
7. 在"系统">"Keeper监控">"运行状态"里重启Keeper
8. 完成升级