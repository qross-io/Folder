# Keeper中的Restful接口

因为Keeper调度程序和Master管理程序是两个独立的程序，一方停机不会影响另一方。但是Master也需要在Keeper运行时能够控制Keeper，即能与Keeper通信。Keeper提供了一些Restful接口用于接收外部的命令或消息。

这些接口需要通过内网访问，IP即本机内网地址，端口号默认为`7700`，可以在“系统”->“Keeper监控”->[“Keeper设置”](/keeper/settings.md)中进行设置，如`http://localhost:7700/kill/task/3306`。另外所有接口的Method均为`PUT`。

以下说明中省略域名和端口。

## 默认主页 `/`

，可以测试Keeper的接口是否能正常工作。

## 环境变量设置 `/global/set?name=&value=`

目的是对环境变量修改不重新启动Keeper。参数`name`和`value`必须有。因为环境变量设置总是成功，所以返回值始终为数字`1`。

例如重启Keeper的参数设置为 `/global/set?name=QUIT_ON_NEXT_BEAT&value=yes`

## 重启任务 `/task/restrat/{taskId}?more=&starter=`

参数列表：

* 路径参数`taskId`必须传，任务ID。
* 地址参数`more`必须有，可选值有：
    + `WHOLE` 重启整个任务，生成新的任务记录。
    + `EXCEPTIONAL` 从DAG中异常的Action重新启动，已执行完的Action不再运行。不生成新的任务记录。
    + `命令ID列表` 从DAG中指定的Action重新启动，不管指定的Action是什么状态。不生成新的任务记录。
* 地址参数`starter`可选，传递用户ID，是指谁进行了重启操作。默认值为`0`。

返回值及说明：
```json
{
    "id": "任务ID",
    "status": "任务状态",
    "recordTime": "任务记录时间，格式 yyyy-MM-dd HH:mm:Ss"
}
```

## 立即运行任务 `/task/instant/{jobId}?creator=&delay=&ignore=`

这个接口立即生成新的任务记录并运行。

参数列表：

* 路径参数`jobId`必须有，调度ID。
* 地址参数`creator`可选，指谁运行了任务，默认值为`0`。
* 地址参数`delay`可选，指延时多少秒再运行任务，默认值为`0`，最大`60`秒。
* 地址参数`ignore`可选，指是否忽略前置依赖，默认值为`no`，另一个值为`yes`。

返回值及说明：
```json
{
    "id": "任务ID",
    "recordTime": "任务记录时间，格式 yyyy-MM-dd HH:mm:Ss"
}
```

## 杀死正在运行的任务 `/kill/task/{taskId}`

这个接口会杀掉正在运行的任务并返回被中断的Action ID列表。路径参数`taskId`必须有。如果指定的Task没有运行，则返回空数组。

返回值示例：
```json
{ "actions": [2035, 2036] }
```

## 杀死正在运行的调度的所有任务 `/kill/job/{jobId}`

这个接口会杀掉指定调度的所有任务副本，路径参数`jobId`必须有。返回值同上个接口。如果指定的Job没有运行的Action，则返回空数组。

## 杀死正在运行的Action `/kill/action/{actionId}`

这个接口会杀掉正在运行的Action，路径参数`actionId`必须有，返回值为：
```json
{ "actions": 2035 }
```
其中`2035`是`actionId`，如果指定的Action没有运行，则返回`0`。

---
参考链接

* [Keeper设置](/keeper/settings.md)