# OneApi 管理功能

在 [OneApi 类](/oneapi/class.md)中，列出了所有 OneApi 相应的方法。除了`signIn`和`request`两个方法用于接口请求外，其他的方法都是管理方法。这些管理方法需要自己绑定到 URL 上的，可参照[示例项目](/oneapi/example.md)。

```java
    @RequestMapping("/oneapi/refresh")
    public int OneApiRefresh(@RequestParam(value = "key") String key) {
        if (OneApi.authenticateManagementKey(key)) {
            return OneApi.refresh();
        }
        else {
            return -1;
        }
    }
```

上例是刷新所有接口的方法：在基于数据库的接口管理方式中，接口更新之后，无需重新构建部署项目，直接调用刷新接口即可。其他管理方法是否映射到URL地址视业务情况而定，注意调用管理功能接口必须传递管理密码。例如：`http://localhost:8080/oneapi/refresh?key=123456`

在作者的开发列表中，OneApi 还有很多功能未开发，如接口访问统计等。将来这些功能都会在管理方法中实现，管理功能会越来越丰富起来。

---
参考链接 

* [OneApi 类](/oneapi/class.md)
* [OneApi 全局设置](/oneapi/setup.md)