# Keeper 自定义事件

在 Keeper 预设的事件中，只有发送邮件、请示接口、执行指定的程序（脚本）或重启任务。显然满足不了常规的业务需求，比如打电话、发短信、发送企业微信或钉钉消息等。虽然可以通过执行指定的程序实现，但是如果每一个调度作业都配置一次又太麻烦了。Keeper 提供了自定义事件的方法，可以把自定义事件理解为是一个“函数”。能够通过程序扩展是 Keeper 与其他调度工具的最大区别。在“系统”->“Keeper 监控”->“自定义事件”中找到自定义事件页面。

自定义事件的各个参数解释如下：

## 事件标签

**事件标签**是显示在调度作业事件列表中的名称，用于说明事件的功能。可以通过输入框中的默认值的方式定义双语：

```json
{ "chinese": "中文标签", "english": "English Label" }
```

不是这种 Json 格式的即为单语言形式，根据需要选择。

事件名称可以是“拨打电话给”、“发送企业微信消息给”（用户信息中包含了企业微信字段）、“发送短信给”等等。

##  事件参数类型

可以向自定义的事件传递参数，这些参数可以传递给实现事件逻辑的程序。参数类型可以是：

* 无参数，即不传递任何参数。
* 使用角色选项按钮，即包含所有者、团队负责人、普通管理员和系统管理员这几个角色的选项按钮，可多选。
* 单行输入框，可以输入一行信息。
* 多行输入框，可以输入多行信息。
* 选项按钮或下拉框，暂时不支持。

## 事件的应用范围

事件应用范围指在哪些任务状态下可以触发这个自定义事件，可以多选。

## 实现事件逻辑的程序

自定义事件支持使用 PQL 过程、Shell 命令或 Python 脚本来实现。如果使用 Java 书写逻辑，可以先打成 jar 包，然后使用 Shell 调用。

在事件逻辑编码过程中，可以使用一些内置的参数或变量，分别说明如下：

* `job_id` 调度作业的ID。
* `title` 调度作业的标题。
* `task_id` 任务ID。
* `status` 任务状态。
* `task_time` 任务创建时间，格式为`yyyyMMddHHmmss00`。
* `record_time`  任务每次执行的时间，格式为`yyyy-MM-dd HH:mm:ss`。
* `event_limit` 事件触发限制条件，由逗号分隔的多个值，比如`auto_start,manual_start`。可选值`4`个值分别有`auto_start`、`manual_start`、`auto_restart`、`manual_restart`，分别对应自动启动、手工启动、自动重启和手工重启，意义详见[调度作业事件](/keeper/event.md)中的说明。
* `value` 事件参数的值。如果是角色选项，是由逗号分隔的多个值，比如`_OWNER,_LEADER`，最多可选`4`个值，分别为所有者`_OWNER`，团队负责人`_LEADER`，普通管理员`_KEEPER`和超级管理员`_MASTER`。

这些内置的值可以作为参数和变量调用。如果以参数形式调用，可使用格式`#{name}`，如`#{task_id}`；如果以变量形式调用，可使用格式`$name`，如`$task_id`。建议在 PQL 过程中使用变量格式调用，在 Shell 或 Python 中使用参数格式调用。

在事件参数类型为“角色选项”时，还支持额外的一些参数或变量：

* `owner` 所有者详细信息
* `leader` 团队负责人详细信息
* `keeper` 普通管理员详细信息
* `master` 超级管理员详细信息
* `users` 所有选中角色的详细信息

以上几个变量类型均为数据表格。当事件参数值选中对应的角色时，其相应变量的表格中就会存有用户的详细信息；如果没有选中某个角色，其对应变量的表格为空。这几个表格的字段名有：

* `id` 用户在 Qross 系统中的唯一id
* `username` 用户名，同登录名，如`zhangsan`
* `fullname` 用户全名，一般为中文名称，如`张三`
* `email` 邮件地址，如`zhangsan@company.com`，发送邮件时可以作为收件地址使用
* `personal` 带用户名的邮件地址，格式为`用户全名<邮件地址>`，如`张三<zhangsan@company.com>`，发送邮件时可以作为收件地址使用
* `mobile` 手机号码
* `wechat_work_id` 企业微信id

在 PQL 中这几个用户变量可以直接使用或遍历，如

```sql
FOR $user OF $users LOOP
    PRINT $user.mobile;
END LOOP;
```

在 Shell 或 Python 中可使用[嵌入式 PQL](/pql/embedded.md) 进行值加工然后传递，如

```sh
java -jar custom-event.jar <%= $owner COLUMN 'mobile' JOIN ',' %>
```

上例中`<%= $owner COLUMN 'mobile' JOIN ',' %>`表示将表`$owner`中`mobile`列的值连成一个由逗号分隔的字符串。不仅可以输出值，也可以通过[嵌入式 PQL](/pql/embedded.md) 实现更复杂的逻辑。


## 自定义事件示例

建议使用 PQL 编写自定义事件逻辑，以下几个由 PQL 写成的事件示例可供参考。

### 请求接口示例

自定义事件时，接口操作用得最多。如短信、电话语音、企业微信、钉钉等都要请示接口。

```sql
REQUEST JSON API '''http://www.domain.com/api/event?job=$job_id''' METHOD 'POST';
```

电话语言接口示例：

```sql
REQUEST JSON API '''https://api.company.com/phone/call?calledShowNumber=01086483133&calledNumber=${ $users COLUMN 'mobile' JOIN ','  }&ttsCode=TTS_169899687&deptNo=0007&token=7f0e43a86e15a20bdff07a160e0b9cc4&cId=9999&name=SingleCallByVoice&jobId=$job_id&jobTitle=${ $title URL ENCODE }''' METHOD 'POST';
```

### 发送邮件示例

发邮件功能在默认事件中已实现。

```sql
SEND MAIL ${ 'ERROR: ' + $title }
    USE TEMPLATE '''@QROSS_HOME/templates/email/alert.html'''
    TO ${ $owner COLUMN 'personal' JOIN ';' }
    CC ${ $leader COLUMN 'personal' JOIN ';' }
    CC ${ $keeper COLUMN 'personal' JOIN ';' }
    BCC ${ $master COLUMN 'personal' JOIN ';' };
```

### 发送企业微信消息示例

```sql
IF @WXWORK_TOKEN == '' OR  @WXWORK_TOKEN_LAST_GET_TIME == '' OR @WXWORK_TOKEN_LAST_GET_TIME EARLIER @NOW >= 7200 SECONDS THEN
		REQUEST JSON API 'https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=@CORPID&corpsecret=f9ekY5w52_2FmVSsFu8MIqp82xgc4FG5mXMnXl8ma_Q';
		VAR $info := PARSE '/' AS ROW;
		IF $info.errcode == 0 THEN
			SET @WXWORK_TOKEN := $info.access_token;
			SET @WXWORK_TOKEN_LAST_GET_TIME := @NOW;
		END IF;
END IF;

IF @WXWORK_TOKEN != '' THEN
    REQUEST JSON API '''https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token=@WXWORK_TOKEN'''
        METHOD "POST"
        SEND DATA {
            "touser" : ${ ["zhangsan", "lisi"] JOIN "|" },
            "msgtype" : "text",
            "agentid" : 1000086,
            "text" : {
               "content" : "快递到啦，请携带工卡前往公司前台领取！"
            }
        };
    OUTPUT # PARSE "/" AS ROW;
END IF;
```

---
参考链接

* [事件和预警 Event](/keeper/event.md)
* [调度作业 Job](/keeper/job.md)
* [嵌入式 PQL](/pql/embedded.md)