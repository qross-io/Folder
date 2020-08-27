# OneApi 快速入门

## 运行环境
OneApi基于Spring Boot框架运行，通过依赖的方式引入到Java Web项目中。和其他方式开发的接口一样通过HTTP方式调取，OneApi只是一种新的接口内容的开发方式，并不改变Spring Boot的任何东西。

## 从示例项目快速开始

示例项目源码地址：

<https://github.com/qross-io/OneApi>

示例项目的环境要求如下：

* JDK 1.8或以上版本
* Gradle 4.9或以上版本（可修改为Maven，见下面的说明）

作者建议使用`Intellij Idea`作为PQL的开发环境，需要`Intellij Idea 2018`或以上版本。

示例项目中已经预先添加了一些文件和配置：

* `ApiController` 包含OneApi的所有地址设置，可自己修改地址映射或添加Controller逻辑。
* `conf.properties` 包含所的有OneApi设置项，需要根据业务场景进行修改。
* `example.sql`和`hello.sql` 两个简单的接口示例文件。

## 将OneApi加入到已有的项目中
在已有的Spring Boot项目中使用OneApi非常容易，只需要两步操作，然后就可以开发接口了。

### 引入OneApi依赖
OneApi的依赖和PQL是同一个依赖。

#### Maven配置如下

1. settings.xml中配置凭证
    ```xml
    <servers>
    <server>
        <id>rdc-releases</id>
        <username>PtbpNI</username>
        <password>kwCz3C0wHx</password>
    </server>
    <server>
        <id>rdc-snapshots</id>
        <username>PtbpNI</username>
        <password>kwCz3C0wHx</password>
    </server>
    </servers>
    ```
2. porn.xml仓库配置
    ```xml
    <distributionManagement>
    <repository>
        <id>rdc-releases</id>
        <url>https://packages.aliyun.com/maven/repository/2011186-release-Aa5YmC/</url>
    </repository>
    <snapshotRepository>
        <id>rdc-snapshots</id>
        <url>https://packages.aliyun.com/maven/repository/2011186-snapshot-FSoDsK/</url>
    </snapshotRepository>
    </distributionManagement>
    ```
3. porn.xml依赖配置
    ```xml
    <dependencies>
        <dependency>
            <groupId>io.qross</groupId>
            <artifactId>pql</artifactId>
            <version>0.6.3-68-SNAPSHOT</version>
        </dependency> 
    </dependencies>
    ```

#### Gradle配置如下
```groovy
repositories {
    maven{
    credentials {
        username 'PtbpNI'
        password 'kwCz3C0wHx'
    }
    url "https://packages.aliyun.com/maven/repository/2011186-snapshot-FSoDsK/"
    //url "https://packages.aliyun.com/maven/repository/2011186-release-Aa5YmC/"
    }
}

dependencies {
    compile (group: 'io.qross', name: 'pql', version: '0.6.4-34-SNAPSHOT')
}
    ```

### 添加RestController
新建一个类并添加`@RestController`注解，也可以使用现有的RestController类。在类中添加一个方法：
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
在resources目录下新建文件夹`api`，然后新建文件`example.sql`。打开`example.sql`，然后添加以下内容：
```sql
## test | GET |
OUTPUT "HELLO WORLD!";
```
这个接口就完成了，运行项目，可以浏览器中输入接口地址`http://localhost:8080/api/example/test`（端口根据你的项目设置确定）查看结果。在示例项目中已经有这个接口。


### 获取更多信息
* 添加数据源配置，请参阅[数据源配置](/pql/properties.md)
* OneApi全局配置，请参阅[OneApi设置](/oneapi/setup.md)
* 关于接口文件存放目录与地址映射的对应关系，请参阅[接口文件组织](/oneapi/file.md)
* 关于接口的返回值问题，请参阅[接口创建与编辑](/oneapi/edit.md)和[OUTPUT语句](/pql/output.md)
* 接口访问的鉴权问题，请参阅[配置Token](/oneapi/token.md)
* OneApi接口与登录用户问题，请参阅[OneApi登录用户](/oneapi/signin.md)
* OneApi类的重载方法，请参阅[OneApi类](/pql/class.md)