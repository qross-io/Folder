# 轻代码开发方法概览

轻代码开发方法（Cogo）的目标是创造一套快速构建应用系统的集成开发工具，属于软件工程领域的研究课题。如何通过技术的手段简化开发流程，提高开发效率，节省开发成本，保证开发质量是设计这个计划的目的和初衷。无论前端开发还是后端开发，轻代码开发方法都提供了更高效的开发手段。

**Cogo** 可以说是一种开发理念，是一种用最少的代码完成 Web 交互功能的一种思想，Cogo 的最终目标是开发出一个能实现“积木式编程”的工具或模型。**Cogo** 中的`Co`表示`Component`，`go`取自`Lego`的`go`，表示“组件积木”的意思，同时也是为了致敬派珀特发明的 **LOGO** 编程语言（Scratch 语言的灵感来源）。Cogo 期望通过一种搭建积木的手段来构建 Web 应用，而且积木的“形状”可以自定义，就像沙盒游戏一样，创造自己的世界。

但是在现有条件下，用积木的方式构建应用，完全不写任何代码，还是一个几乎不可有的任务。于是先退而求次，可以先实现“尽可能少写代码”。但是“不写代码”仍是 Cogo 努力的方向。

**Cogo 的先期目标是创建无 Java、几乎无 Javascript、几乎无 CSS 和少量 HTML 的基于 Markdown 文件格式的交互页面和应用。**

## Cogo 功能特点

Cogo 本质上属于低代码的研究方向，但与其他的低代码产品不同，编写更简单，场景也更通用。

1. 不需要写 Java 代码。虽然 Cogo 项目都基于 Spring Boot 环境开发，但是项目开发过程中不需要编写 Java 代码，实现所有业务逻辑的都可以通过 PQL 完成。极端情况下，项目仍然可以用 Java 实现后端逻辑。

2. 不需要编写 Javascript 代码。在应用系统中，Javascript 是实现页面交互的唯一选择，Cogo 封装了大量组件，可以实现无 Javascript 的交互页面编程。这些组件都支持通过属性进行配置。和后端一样，前端并不禁止使用 Javascript 进行编码。

3. 一般不需要写 CSS。项目使用主题进行样式控制，将来会提供多个主题供开发者选择。

4. 基于 Markdown 而不是 HTML 格式进行页面总局，Markdown 比 HTML 格式要简单很多，在编写过程中可以和 HTML 标签交互使用。Cogo 提供了更多 Markdown 语法扩展以满足更多的格式或样式控制。

5. 多种模板和框架选择，菜单和功能项可编辑。

6. 通过图形界面在线即时编辑，即时应用，无需要重启系统，无需重新部署。

7. 支持各种方式的身份认证，标准登录、SSO 单点登录、LDAP 认证、企业微信认证、钉钉认证等。

8. 将来会提供用户访问及行为统计分析功能，方便管理员了解整个系统的访问和运行情况。

9. 方便在系统内进行各种系统配置，即时生效。

10. 其他 PQL、Voyager、OneApi、root.js 等模块支持的所有特性。

更多功能正在不断研究和开发中...

## Cogo 技术构成

Cogo 是整个 Qross 系统各个组件模块的综合运用。Cogo 使用了以下技术：

1. Cogo 是 Java 开发的一个 Spring Boot 项目，可以方便的在各种环境下进行部署。
2. [PQL](/pql/overview.md) 是 Cogo 中使用最多的语言，无论在前端还是后端。
2. Cogo 后端部分使用 [OneApi](/oneapi/overview.md) 进行接口管理。
3. Cogo 使用 [Voyager](/voyager/overview.md) 作为模板引擎以方便从后端交换数据。
4. Cogo 前端部分使用 [root.js 标签库](/oneapi/overview.md)，方便快速构造交互页面。
5. Cogo 使用 [Marker](/voyager/marker.md) 来转化 Markdown 文档格式。

---
参考链接

* [PQL 过程化查询语言](/pql/overview.md)
* [OneApi 统一接口管理](/oneapi/overview.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [root.js 前端标签库](/root.js/overview.md)

