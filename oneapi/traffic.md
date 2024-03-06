# OneApi 流量统计

（此功能已弃用）

流量统计可以为每个接口的访问进行记录，并会自动生成相应的图表，可细化到每个参数和值的访问量。如果图表不满足需要，还可以自行进行扩展。

## 流量统计相关设置

如果要使用 OneApi 的流量统计功能，必须先进行以下设置。

* 接口服务的名称必须设置，参照[接口服务](/oneapi/service.md)。
* 需设置 Qross 系统的数据连接，连接名`mysql.qross`，参照[数据源配置](/pql/properties.md)。
* 需要 Qross 系统中与 OneApi 流量功能相关的数据表，部署 Qross 系统会自动创建：
    + `qross_api_in_one` 保存所有数据接口
    + `qross_api_service` 保存所有接口服务
    + `qross_api_traffic_summary` 保存流量汇总数据
    + `qross_api_traffic_details` 保存流量的访问明细，不含参数
    + `qross_api_traffic_queries_summary` 保存流量的参数汇总数据
    + `qross_api_traffic_queries_details` 保存流量参数的访问明细
* 使用 [Qross Master](/master/overview.md) 对接口进行管理，可以查看流量统计报表并且可以进行相应的定制功能。
* 在 Qross Master 在接口服务管理中添加与配置文件中名称一致的接口服务，可以理解为手工绑定。
* 在 Qross Mastr 配置服务的“流量统计级别”设置项，至少不能禁用流量统计，详见下文的配置项说明。

## 流量统计级别

为了满足不同的统计需求，可以设置不同的统计级别。

+ 级别 1，只统计接口的访问量，到小时级别。
+ 级别 2，记录接口的访问明细。
+ 级别 3，统计参数及值的访问量，到天级别。
+ 级别 4，记录每个接口和值的访问明细。


总体来说，OneApi 的统计功能仍相对简陋，会随着版本的更新不断完善。

---
参考链接

* [OneApi 接口文档](/oneapi/document.md)