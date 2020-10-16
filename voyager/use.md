# 使用Voyager

在 Spring Boot 项目中引用 Voyager 非常简单。

## 引入PQL依赖

在使用前需要引用相关PQL依赖并进行相关的数据源设置，请参照[使用PQL](/pql/use-pql.md)

## 添加`@Bean`

以 Master 项目为例，代码如下：
```java
@SpringBootApplication
public class MasterApplication {
	public static void main(String[] args) {
		SpringApplication.run(MasterApplication.class, args);
	}

	@Bean
	public VoyagerResolver initVoyagerResolver(){
		return new VoyagerResolver();
	}
}
```

上面是Master项目的入口类，在`main`方法下面的`initVoyagerResolver`方法，即Voyager模板引擎的相关代码。

## Hello World

完成了以上两步，就可以开始编写业务逻辑了。

1. 添加`Controller`，地址是`/home`，映射到页面`home.html`
    ```java
    @RequestMapping("/home")
    String home(Model root, HttpServletRequest request) {
        return "home";
    }
    ```
2. 在项目的`/resources/templates`目录下（没有则新建）添加页面`home.html`
3. 在页面上书写代码（省略了`body`之外的部分）
    ```html
    <body>
        <%=@NOW%> <%='Hello World'%>
    </body>
    ```
    上面代码中，`@NOW`用于显示当前时间。
4. 启动 Spring Boot 项目，在浏览器中输入`http://localhost:8080/home`查看运行结果。


---
参考链接

* [Voyager语法](/voyager/syntax.md)
* [Voyager设置](/voyager/setup.md)