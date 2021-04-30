# 接口返回值的数据结构

所有 OneApi 接口除了返回所请求的数据以外，还会有其他的一些描述信息。一个标准的 OneApi 接口格式如下：

```json
{
    "code": 200,
    "status": "success",
    "message": "",
    "data": [],
    "timestamp": "2021-04-19 23:12:11",
    "elapsed": 50
}
```

-- 10 --

其中各个部分的解释如下：

* `code` 接口的结果 Http 编码，其中`200`对应成功，`500`对应程序错误，`403`表示无权限访问，`404`代表地址错误等。
* `status` 接口的结果状态，其中`success`表示成功，其他状态还有`exception`表示接口出错，`denied`表示鉴权失败，`incorrect-path-or-methoo`表示错误的路径或请求方法。
* `message` 接口的状态为`success`时，这一项为空值；当状态不为`success`时，这一项会显示具体的出错信息。
* `data` 成功状态下接口逻辑返回的数据，其他状态为`null`。即只有成功时，这个接口才是可用的。
* `timestamp` 接口请求的时间。
* `elapsed` 接口执行的时长，单位为毫秒。

---
参考链接

* [创建和编辑接口](/oneapi/edit.md)
* [向 OneApi 的接口传递参数](/oneapi/params.md)