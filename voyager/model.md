# 通过 model 向页面传递数据

虽然在页面上可以通过 PQL 从各种地方获取数据，但是有时我们仍需要通过 Spring Boot 的 Controller 向页面传递数据，就像其他的模板引擎那样。Voyager 也支持通过 model 向页面传递数据，传递的数据会自动生成 PQL 的用户变量。

```java
@RequestMapping("/carton/page")
String page(Model root, @RequestParam(value = "id") int id, HttpServletRequest request) {
    root.addAttribute("name", "Tom");
    root.addAttribute("age", 18);
    return "/carton/page";
}
```

以上示例中，Voyager 会自动将 model 中的两个参数`name`和`age`自动转成 PQL 变量`$name`和`$age`，可以直接在页面上的 PQL 代码中使用。

虽说 Model 中支持任意类型的数值，但建议传递的数据类型只包括基础数据类型和复合类型 List （或 ArrayList ）、 io.qross.core.DataRow、 io.qross.core.DataTable。这个问题是由于 Java 和 PQL 的数据类型不一致引起的，不过可传递的数据类型已经能满足很多需求。

---
参考链接

* [Voyager 处理 URL 中的参数](/voyager/query.md)