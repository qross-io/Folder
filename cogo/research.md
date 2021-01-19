## Cogo 实现

能否在 Vue 或 React 实现 Cogo 思想

## Cogo 基础

是否可以在特定的领域实现积木编程，比如管理信息系统。
负载均衡

### 精简思路

减少组件的属性，扩展已有的组件属性，同一属性实现更多的功能
```
<input id="Age" type="int" required-text="Please input your age." />
```

以上例说明，`input`尽可能不填写其他属性，使用系统默认值。`type`的属性值`int`本来没有，即在原有的基础上进了扩展。`requrired`属性包含两个功能，一是这个输入框的信息必填，另一个如果输入框为空则提示`Please input your age.`。

### 运行环境

由于需要与数据库或其他数据源进行交互，因此编写 SQL 语句是少不了的，即使能做到像 Hibernate 或其他数据层框架那样自动映射，也完全避免不了其他的后端逻辑。PQL 可以完成所有后端逻辑，缺什么加什么就好。

由于 PQL 需要运行在 JVM 虚拟机上，所以把非常流行 **Spring Boot** 作为 Cogo 的运行环境。

### 开发重点

要想让 Cogo 工作，要做的有三件事：第一个是“内容”，是 Cogo 的关键，内容包含页面和系统；第二个是 Cogo 开发环境，用来生产内容；最后一个是运行 Cogo 内容的环境。比如 Dreamweaver 生产 html 网页，浏览器运行 html。当然，这两个环境也可以是同一个，比如 Excel。

因为暂时不需要“所见即所得”的开发环境，所以内容和运行环境就成了开发重点，开发环境暂时使用 Visual Studio Code 等工具代替。

### Cogo 样式

CSS 可以控制页面和元素的样式，但是为了尽可能的不写样式代码，可以使用主题来代替样式控制（CSS），即预选设定好的样式。主题可以预先创建多套，每套主题一套样式风格，Qross 系统已经可以实现 32 种颜色风格的随机切换，可以套用过来。

### Cogo 组件

“组件”是积木编程的基本单位，组件是一个或多个元素（或标签）的集合，并且有自己的样式、属性、方法和事件。例如：

```html
<input type="text" id="TextBox1" />
<button id="Button1" onclick="$s('#TextBox1').focus()"></button>
```

以上代码中，`input`或`button`是元素标签名，`focus`是方法，`onclick`是事件。在 Cogo 中，因为方法只能通过编程的方式调用，所以要尽可能的消除方法。

丰富的组件是实现积木编程方式的前提，并且组件要提供交互功能，以满足系统要求。而且，系统内置组件不可能满足所有需求，用户可以自定义组件。以下是一些可以自定义或扩展 Html 标签的经验：

* 可以定义自己的 HTML 标签，如 `<clock></clock>`。浏览器不会解析这些自定义的标签，需要自己编写解析逻辑。自定义标签可以自已添加属性、方法和事件。对于自定义标签，浏览器但也不会显示出来。
* 对于浏览器支持的标签，可以添加自定义属性，这个属性不会被浏览器解析和处理，需要自行处理，也可以作为额外保存数据的手段。例如：`<input focus-value="Hello World" />`。通过标签的`setAttribute`和`getAttribute`方法可以设置或获取自定义属性。
* 对于浏览器支持的标签属性，可以自定义属性值，如`<input type="int" />`，但是这个值也需要自行处理。
* 浏览器支持的标签不可以添加自定义方法和自定义事件， 一般情况下不需要。

页面与用户交互最主要的是依靠组件的事件，一般情况下触发事件通过 Ajax 调取后端接口，接口中实现后端逻辑。例如：
```html
<button id="Button1"> Submit </button>
<script type="text/javascript">
$s('#Button').onclick = function() {
    $POST('/api/test?id=$(id)&name=${name}')
        .success(data => {
            window.location.href = '/page1';
        });
}
</script>
```

为了达到尽可能少写代码的目的，需要把以上代码进行缩减，可以这样：

```html
<button onclick="/api/test?id=$(id)&name={name}" jump-to="/page1"> Sumbit </button>
```

*上面要调取的接口可以跨域。

再简化些，甚至可以干掉后端接口：

```html
<button onclick="update table1 set name=#{name} where id=#{id}" jump-to="/page1"> Sumbit </button>
```

对于页面初始化时，也可以通过接口或 PQL 语句初始化数据，例如：
```html
<select data="select name, id from table1"></select>

<table data="select * from table2 -> first row"></table>
```

可以说，缩短获取数据或交互流程是减少代码量的关键点。


### Cogo 页面

为了最大化减少代码和复杂性，需要把所有的逻辑尽可能的放在一个页面上来，尽可能减少分层或者分页面。

* 使用 MarkDown 作为 Cogo 页面的源代码。MarkDown 是一种语法非常简单的文档格式，编写内容极其方便。MarkDown 可以无障碍转化成 HTML，且在编辑时支持与 HTML 代码混合。
* Voyager 模板引擎可以通过嵌入式 PQL 在页面上编写服务器端代码。需要升级 Voyager 模板引擎以支持 MarkDown 文档的转换。不过尽量减少交叉编写客户端和服务器端代码。



基于以上两个条件，所有只剩下两个关键逻辑还未实现，一是初始化页面组件时尽量不使用服务器端代码以保证逻辑清楚，二是交互事件尽可能简单，以避免写 Javascript 代码。

### Cogo 系统
一个页面包含所有交互逻辑，多个页面构成应用

标准登录，LDAP，SSO
企业微信插件、钉钉插件、小程序
用户系统


框架布局也可以使用主题控制
组件上加载数据可以直接执行 PQL，可以直接调取接口（含跨域接口）
客户端事件可以直接执行 PQL，可以直接调取接口（含跨域接口）

独立的编辑平台，前期可以只提供代码编辑，以后慢慢制作图形化界面
发布后的系统在修改Bug或增加功能时无需重启
创建的系统由一个 Spring Boot 解析项目构成，创建的页面保存在项目之外，通过项目配置加载
支持国际化，由 Voyager 提供支持
更新无需要重启系统，页面即配置，自由修改
修改器可以连接不同系统

## 适用场景
可适用于文档管理等场景


<# page template="/template/form.html?previous=上一级&button=#bulid#&button=New Class&button_class=new&crumb=研究进度&jump=/user/user%3ffrom%3dindex" />


# 研究进度

如之前讨论过，设计的目标是将前端甚至后端开发整合到一个页面中来，就是这个页面要同时可以进行前端和后端开发。现在研究进度汇总如下：

1. 使用 MarkDown 作为文件的源代码，可以在访问时生成 HTML。这个已开发完成。

2. 使用主题来实现样式，同时适配 PC 和移动端。这个实现较容易，暂时未做，优先级较低。

3. 使用自定义HTML标签来封装组件，而不是使用 Javascript 来呈现组件，思路可行。例如
    ```html
    <calendar></calendar>
    ```
   这个需要大量组件支持，但并不是不可实现的。
   
4. 服务器端向页面端传递数据，使用 PQL 嵌入式，不再需要写 Java。例如
    ```html
    <~u0025=SELECT id, title FROM qross_jobs -> TO HTML TABLE ~u0025>
    <~u0025 FOR $student IN (SELECT * FROM students) LOOP ~u0025>
        <div><~u0025=$student.name~u0025></div>
    <~u0025 END LOOP ~u0025>
    ```
    现有可实现的功能，不需要开发。服务器端向页面端发送数据是在用户看到页面之前，如果发送的数据太多或逻辑太复杂会影响页面的加载，而且这种开发方式虽然很好用但是有些不友好，比如容易出错。

5. 向前端页面异步传递数据，例如：
    ```html
    <table data="SELECT * from students">
    </table>
    <for var="student" in="http://www.domain.com/api/students -> /data">
        <div>@[name]</div>
    </for>    
    ```
    这种方式是在页面加载完成之后才进行数据加载和组件渲染，想对来说比上一种友好一些，但灵活性比上一种差一些。可以两种方式混合使用。实现进度80%。

6. 客户端交互事件，这是研究中最困难的部分。在上面的几项中，没有出现 Javascript 代码，但“交互”与上两条的“仅加载”不同，交互需要通过执行客户端事件来实现相应的功能。目前考虑的方向有几个：
    
    * 使用最简洁的框架实现，例如：
        ```javascript
        $run("update students set name='Tom' where id=#{id}")
            .then(data => ...)
            .get("http://www.domain.com/api/students -> /data")
            .....
        ```
       这个框架可以同时编写服务器端代码和客户端代码，实现可能性100%。缺点是还是要写 Javascript，离设计的初衷还有一段距离。
    * 使用另一种高度集成的语言代替 Javascript，例如：
        ```
        get: http://domain.com/api -> /data;
        jump: /abc?id=#{/data/0};
        class: #id -> className;
        style: #id -> color=a&fontSize=b;
        show: #id;
        pql: {
            select * from table1;
            update abc set a=1 where id=1;
        };
        call: abc(1, 2, 3, 4);
        alert: abc;
        open: popup#abc;
        ```
       在后端开发中，PQL 也是一种高度集成的语言，替代了 Java 95% 以上的工作，如果预设好场景，比如小程序开发，则可以完全代替 Java。上面的设计也是这种思路，类似于30年前的 Basic 或汇编。
       设计中去掉了数据类型，可以混合编写接口调用、SQL语句和客户端功能。但是复杂功能难以实现，如循环、条件判断等。
    * 事件调用可以写在标签上，如：
        ```html
        <button onclick="update students set name='Tom' where id=#{id}"></button>
        <button onclick="http://www.domain.com/api/students; jump: /page/list"></button>
        ```
       但这种方式非常不灵活，而且需要服务器端去解析，资源消耗较大。
    
    个人倾向于使用第一种方式，第二种方式的时机还不成熟或还没想透。
    
    <% FOR $i IN 0 TO 10 LOOP %>
        <div>${i}</div>
    <% END LOOP %>

总结一下现在的问题就是在没有交互的页面中，已经可以最简化甚至不需要写 Javascript 代码，但是有交互功能页面还没有一个特别完美的方式去掉 Javascript。