# OneApi 接口文档

接口使用者比如前端开发人员可以根据文档了解接口的详细信息，以确定如何调用这个接口。OneApi 提供了文档定义功能，也提供了简单的文档查阅页面。

## 必要的设置

如果要使用 OneApi 的接口文档功能，必须先进行以下设置。

* 接口服务的名称必须设置，参照[接口服务](/oneapi/service.md)。
* 需设置 Qross 系统的数据连接，连接名`mysql.qross`，参照[数据源配置](/pql/properties.md)。
* 需要 Qross 系统中与 OneApi 文档功能相关的数据表，部署 Qross 系统会自动创建，也可手工创建：
    + `qross_api_in_one` 保存所有数据接口
    + `qross_api_service` 保存所有接口服务

## 在接口添加注释

在基于文件的接口管理中，文档内容通过注释方式添加。例如：

```sql
/*
 * 删除自定义函数
 * 在 Keeper 运行时会同时更新 Keeper，只能删除自己的函数
 * #id INT 函数的 id
 * #owner INT 所有者的用户 id
 * @return INT 受影响的行数
 * @create Tom 2021-4-27 
 * @update Jerry 2021-4-28
 */
## function | DELETE | id=0 |
DELETE FROM qross_functions WHERE id=#{id} AND owner=#{owner};
```

上例为 Master 项目中的一个接口，其中注释部分及接口的说明，系统会自动解析这些信息生成文档的各个部分。分别说明如下：

* 必须以多行注释的形式进行定义，注释需定义在接口声明的上方。
* 除了注释开始标志`/*`和结束标志`*/`，注释中的中间部分都是文档内容。
* 每一行开始的`*`号可以省略。
* 第一行是接口的标题，即简要的说明文字。
* 第二行是接口的详细描述，如果一行写不下，描述可以写多行，描述也可以省略。
* 参数以井号`#`开始，需与接口逻辑中使用的参数名称一一对应，在参数名称之后需要添加一个空格。如上面的`#id INT 函数的id`是参数`id`的说明，其中的`INT`表示数据类型，可以省略。
* 参数说明可以写多个，每个参数说明占一行。
* `@return` 返回值信息，后面的格式与参数相同，可以为任意内容。
* `@permit` 授权信息，说明谁可以访问这个接口
* `@created` 创建者、创建时间及其他相关信息
* `@updated` 修改者、修改时间及其他相关信息

## 接口文档查阅

目前在 OneApi 示例项目中有两个页面：`index.html`和`detail.html`，对应的 Controller 设置在`DocumentController.java`文件中。可以在自己的项目中添加这两个文件用于查阅文档，也可以直接运行 OneApi 示例项目来查阅接口文档，访问地址为`http://localhost:7070/oneapi/docs`。

作者会在 Master 项目中提供接口管理的所有功能，当然也包含文档查阅。

---
参考链接

* [OneApi 流量统计](/oneapi/traffic.md)