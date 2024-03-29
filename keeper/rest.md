# Keeper 中的 Restful 接口

因为 Keeper 调度程序和 Master 管理程序是两个独立的程序，一方停机不会影响另一方。但是有时我们需要在 Keeper 运行时能够控制 Keeper，即能与 Keeper 通信。Keeper 提供了一些 Restful 接口用于接收外部的命令或消息。

这些接口需要通过内网访问，IP 即本机内网地址，端口号默认为`7700`，可以在“系统”->“Keeper 监控”->[“Keeper 设置”](/keeper/settings.md)中进行设置，如`http://localhost:7700/kill/task/3306`。另外所有接口的请求方法 Method 如无特别说明，是默认为`PUT`。

在多机模式下，可以在 PQL 使用`@KEEPER_HTTP_SERVICE`全局变量获取任意一个节点的地址进行通信，一般返回是负载最低的那个节点。

除了首页用于测试外，其他所有接口均需要使用 TOKEN 认证权限，TOKEN 在系统中设置，对应的全局变量`@KEEPER_HTTP_TOKEN`。下例是一个典型的接口请求。

```sql
REQUEST '''http://@KEEPER_HTTP_SERVICE/configuration/reload?token=@KEEPER_HTTP_TOKEN''' METHOD 'PUT';
PARSE '/' AS VALUE;
```

以下是各个接口的说明，省略域名和端口。

## 默认主页 `/`

可以测试 Keeper 是否在正常运行，或者接口服务能否在正常工作。

## 环境变量设置 `/global/set?name=&value=`

目的是对环境变量修改不重新启动 Keeper。参数`name`和`value`必须有。因为环境变量设置总是成功，所以返回值始终为数字`1`。

例如重启 Keeper 的参数设置为 `/global/set?name=QUIT_ON_NEXT_BEAT&value=yes`

## 重启任务 `/task/restrat/{taskId}?more=&starter=`

参数列表：

* 路径参数`taskId`必须传，任务 ID。
* 地址参数`more`必须有，可选值有：
    + `WHOLE` 重启整个任务，生成新的任务记录。
    + `EXCEPTIONAL` 从 DAG 中异常的 Action 重新启动，已执行完的 Action 不再运行。不生成新的任务记录。
    + `命令ID列表` 从 DAG 中指定的 Action 重新启动，不管指定的 Action 是什么状态。不生成新的任务记录。
* 地址参数`starter`可选，传递用户 ID，是指谁进行了重启操作。默认值为`0`。

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

* 路径参数`jobId`必须有，调度 ID。
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

## 杀死正在运行的任务 `/task/kill/{taskId}`

这个接口会杀掉正在运行的任务并返回被中断的 Action ID 列表。路径参数`taskId`必须有。如果指定的 Task 没有运行，则返回空数组。

返回值示例：

```json
{ "actions": [2035, 2036] }
```

## 获取任务的运行日志 `/task/logs`

这个接口的请求方法为`GET`。需要的参数如下：

* `jobId` 调度 id
* `taskId` 任务 id
* `recordTime` 任务记录时间，格式`yyyy-MM-dd HH:mm:ss`
* `actionId` 动作 Action 的 id。默认值`0`，表示获取任务的所有日志。
* `cursor` 读取到的记录位置，下面会详细说明。默认值`0`，表示从头开始读。
* `mode` 日志的分类，可选值有全部日志 `all`，调试日志 `debug`，错误日志 `error`，默认值为`all`。

日志以文件的行式保存到运行服务器上，任务在运行时会不断的产生日志。这个接口每次请求最多返回 100 行日志，并且会返回日志读取到的位置。

```json
{
    "logs": [ ... ],
    "cursor": 1457
}
```

如上所示，接口返回值除了有日志数据外，还会返回本次读取到的位置`cursor`。下次请求时，需要传递`cursor`的值，表示从上次读取到的位置开始继续读取日志。


## 杀死正在运行的调度的所有任务 `/job/kill/{jobId}`

这个接口会杀掉指定调度的所有任务副本，路径参数`jobId`必须有。返回值同上个接口。如果指定的 Job 没有运行的 Action，则返回空数组。

## 杀死正在运行的 Action `/action/kill/{actionId}`

这个接口会杀掉正在运行的 Action，路径参数`actionId`必须有，返回值为：

```json
{ "actions": 2035 }
```

其中`2035`是`actionId`，如果指定的 Action 没有运行，则返回`0`。

## 重新加载所有配置 `/configurations/reload`

系统接口。重新加载数据源配置、数据连接配置、PQL 系统函数、PQL 全局变量等全局配置。返回值固定为`1`。

## 重新加载某个连接配置 `/connection/setup/{connectionId}`

系统接口。重新加载指定的数据连接。

## 移除某个数据连接 `/connection/remove/{connectionId}`

系统接口。移除指定的数据连接。

## 重新加载某个系统函数 `/function/renew?function_name=`

系统接口。

## 移除某个系统函数 `/function/remove?function_name=`

系统接口。

## 重新加载某个全局变量 `/variable/renew?variable_name=`

系统接口。

## 移除某个全局变量 `/variable/remove?variable_name=`

系统接口。

---
参考链接

* [Keeper 设置](/keeper/settings.md)