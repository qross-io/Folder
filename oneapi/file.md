# 用文件目录组织接口文件

在 [OneApi 全局设置](/oneapi/setup.md)中，有一个参数叫`oneapi.resoures.dirs`，这个参数设置接口文件的保存目录。OneApi 从这些目录中读取接口信息，用户发起请求起系统根据 **Controller** 设置调取对应路径的接口。现在介绍一下文件目录、Controller 和接口路径的关系。

## 文件目录和接口路径的关系

在指定的 resources 文件目录中，需要创建一些`.sql`扩展名的文件来保存接口，一个文件里可以写多个接口，见[创建和编辑接口](/oneapi/edit.md)。每个接口的路径就是由 **文件夹名**、**文件名**、**接口名**三部分构成。比如文件夹名为`api`，文件名为`example.sql`，接口名为`test`，那么整个接口的路径就是 **`/api/example/test`**。Controller 就是根据接口路径来找到相应的接口并执行。一般情况下，文件名用来表示模块，接口名用来表示功能。如果模块下包含子模块，可以建子级目录，假设在`api`建了一个目录叫`school`，在`school`目录下建了一个文件叫`exam.sql`，在文件`exam.sql`文件中建了一个接口叫`score`，那么接口`score`的全路径就是 **`/api/school/exam/score`**。因为一个接口文件可以保存多个接口，一个文件目录又可以保存多个接口文件，所以在中小型项目中不建议建太多层级的目录，一层最多两层足矣。

## Controller 如何对应接口路径

在[快速入门](/oneapi/quick.md)中我们已经见到了 OneApi Controller 的基本格式。现在解释一下 Controller 如何工作的。

```java
    @RequestMapping("/api/{group}/{name}")
    public Object OneApi(@PathVariable("group") String group, @PathVariable("name") String name) {
        return OneApi.request("/api/" + group + "/" + name);
    }
```

Controller 接收两个路径参数，一个是`group`，另一个是`name`，假设请求的地址路径是`/api/example/test`，那么`group`的值就是`example`，`name`的值就是`test`。在方法内这两个参数被连接成字符串`"/api/" + group + "/" + name`，字符串计算的值就是`"/api/example/test"`，OneApi 根据这个值去调用路径对应的接口。修改映射地址不会影响结果，如将`@RequestMapping("/api/{group}/{name}")`修改为`@RequestMapping("/data/{group}/{name}")`也没什么问题，只不过改变了请求的URL地址，并没有改变映射关系。再举个栗子，计划把URL地址修改成`/data/example-test`，那么 Controller 就可以修改成（假设文件名和接口名中不包含中线`-`）：

```java
    @RequestMapping("/api/{path}")
    public Object OneApi(@PathVariable("path") String path) {
        return OneApi.request("/api/" + path.replace("-", "/"));
    }
```

所以，路径可以自由设置，只要想办法让 URL 地址路径和 OneApi 接口路径对应上即可。也可以把 OneApi 路径信息放在请求参数中，但是不太符合 Restful 接口规范。

```java
    @RequestMapping("/api")
    public Object OneApi(@RequestParam(value = "name") String group, @RequestParam(value = "name") String name) {
        return OneApi.request("/api" + group + "/" + name);
    }
```

以下是多级目录的示例：

```java
    @RequestMapping("/api/{dir}/{group}/{name}")
    public Object OneApi(@PathVariable("dir") String dir, @PathVariable("group") String group, @PathVariable("name") String name) {
        return OneApi.request("/" + dir + "/" + group + "/" + name);
    }
```

另外，你可以根据自己的需要在 Controller 中添加自己的业务逻辑，如用户登录验证等，最后只要调用`OneApi.request()`方法即可。OneApi 提供了两种方式的 Token 认证，详见[访问控制](/oneapi/token.md)。你还可以把用户的鉴权信息传给 OneApi，方便在接口逻辑中调用，详见[登录用户](/oneapi/signin.md)。关于 OneApi 类的介绍和重载方法，详见 [OneApi 类](/pql/class.md);


---
参考链接

* [OneApi 快速入门](/oneapi/quick.md)
* [OneApi 全局设置](/oneapi/setup.md)
* [OneApi 访问控制](/oneapi/token.md)
* [OneApi 登录用户](/oneapi/signin.md)
* [OneApi 类](/oneapi/class.md)