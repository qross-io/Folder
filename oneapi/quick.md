# OneApi 快速入门

## 运行环境

OneApi 基于 Spring Boot 框架运行，通过依赖的方式引入到 Java Web 项目中。和其他方式开发的接口一样通过 HTTP 方式调取，OneApi 只是一种新的接口内容的开发方式，并不改变 Spring Boot 的任何东西。

## 从示例项目快速开始

示例项目源码地址：

<https://github.com/qross-io/OneApi>

示例项目的环境要求如下：

* JDK 1.8或以上版本
* Gradle 4.9或以上版本（可修改为Maven，见下面的说明）

作者建议使用`Intellij Idea`作为 PQL 的开发环境，需要`Intellij Idea 2018`或以上版本。

示例项目中已经预先添加了一些文件和配置：

* `ApiController` 包含 OneApi 的所有地址设置，可自己修改地址映射或添加 Controller 逻辑。
* `conf.properties` 包含所的有 OneApi 设置项，需要根据业务场景进行修改。
* `example.sql`和`hello.sql` 两个简单的接口示例文件。

## 将 OneApi 加入到已有的项目中

在已有的 Spring Boot 项目中使用 OneApi 非常容易，只需要两步操作，然后就可以开发接口了。

## 引入 OneApi 依赖

OneApi 的依赖和 PQL 是同一个依赖，参见[使用 PQL](/pql/use-pql.md) 中关于依赖的配置内容。

## 添加 RestController

新建一个类并添加`@RestController`注解，也可以使用现有的 RestController 类。在类中添加一个方法：

```java
import io.qross.app.OneApi;
import org.springframework.web.bind.annotation.*;

@RestController
public class ApiController {
    @RequestMapping("/api/{group}/{name}")
    public Object OneApi(@PathVariable("group") String group, @PathVariable("name") String name) {
        return OneApi.request("/api/" + group + "/" + name);
    }
}
```

上例中类名`ApiController`和地址映射的路径`/api/`可以根据需要命名，其他地方无需要修改。

## 创建第一个接口

在 resources 目录下新建文件夹`api`，然后新建文件`example.sql`。打开`example.sql`，然后添加以下内容：

```sql
## test | GET |
OUTPUT "HELLO WORLD!";
```

这个接口就完成了，运行项目，可以浏览器中输入接口地址`http://localhost:8080/api/example/test`（端口根据你的项目设置确定）查看结果。在示例项目中已经有这个接口。

## 获取更多信息

* 添加数据源配置，请参阅[数据源配置](/pql/properties.md)
* OneApi 全局配置，请参阅 [OneApi 设置](/oneapi/setup.md)
* 每个接口都有一个唯一的路径，关于接口文件存放目录与地址映射的对应关系，请参阅[接口文件组织](/oneapi/file.md)
* 关于接口的返回值问题，请参阅[接口创建与编辑](/oneapi/edit.md)和[OUTPUT语句](/pql/output.md)
* 接口访问的鉴权问题，请参阅[配置Token](/oneapi/token.md)
* OneApi 接口与登录用户问题，请参阅[OneApi登录用户](/oneapi/signin.md)
* OneApi 类的重载方法，请参阅[OneApi类](/pql/class.md)