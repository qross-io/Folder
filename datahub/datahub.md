# DataHub 类

文档正在补全中....

**DataHub** 类是整个框架个核心类，框架的操作都是从这个类开始。这个类有大量的方法和相关扩展，包含数据操作的方方面面。DataHub 也是 PQL 的执行引擎。

## 构造函数

* `DataHub()`
* `DataHub(defaultConnectionName: String)`

示例代码：

```scala
val dh = new DataHub() // 默认打开 jdbc.default 连接
val dh = new DataHub("mysql.test")
val dh = DataHub.DEFAULT // 默认打开 jdbc.default 连接
......
dh.close()
```

## 核心逻辑



## 打开数据源

* `open(connectionNameOrDataSource: Any): DataHub`
* `open(connectionNameOrDataSource: Any, defaultDatabaseName: String): DataHub`
* `open(connectionName: String, driver: String, connectionString: String, username: String, password: String): DataHub`
* `open(connectionName: String, driver: String, connectionString: String, username: String, password: String, databaseName: String): DataHub`
* `openDefault(): DataHub`
* `openCache(): DataHub`
* `openTemp(): DataHub`

## 指定目标数据源

* `saveTo(connectionName: String): DataHub`
* `saveTo(connectionName: String, defaultDatabaseName: String): DataHub`
* `saveTo(connectionName: String, driver: String, connectionString: String, username: String, password: String): DataHub`
* `saveTo(connectionName: String, driver: String, connectionString: String, username: String, password: String, database: String): DataHub`
* `saveToDefault(): DataHub`
* `saveToCache(): DataHub`
* `saveToTemp(): DataHub`