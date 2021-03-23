# PQL 源码和示例项目

## PQL 源码

PQL 使用 Scala 和 Java 开发，免费使用且代码开源。项目代码保存在 github 上，地址为：  

**<https://github.com/qross-io/PQL>**


## PQL 示例项目

作者提供了一个 PQL 示例项目，适用于数据脚本开发，后端接口开发见 [OneApi 示例项目](/oenapi/example.md)。示例项目 github 地址为：

**<https://github.com/qross-io/PQLExample>**

项目开发环境要求：

* Intellij Idea 2018或以上版本（强烈建议）
* JDK 1.8或以上版本（必须）
* Scala 2.12 或以上版本（不必须，但建议有）
* Gradle 4.9 或以上版本（可自行修改成Maven）

项目内配置了 Java 入口类`io.qross.Main`和 Scala 的入口类`io.qross.Test`和运行 PQL 过程文件的代码示例。数据源配置在资源目录`resources`下的`conf.properties`文件中，可根据自己项目需要修改或添加数据源。PQL 过程文件保存在资源目录`resources`的`pql`文件夹下，包含两个简单的示例。更多功能请参阅 PQL 的其他文档。

项目开发的 PQL 过程文件可以在服务器上通过 **[Worker](/worker/overview.md)** 运行，服务器上只需要安装`JDK 1.8`即可。因为只是运行文件，所有项目无需要编译。调度程序可通过 **[Keeper](/keeper/overview.md)** 完成，Keeper 提供了强大完善的调度功能，且与 PQL 结合紧密，通过 PQL 可实现很多的自定义功能，甚至可以直接调度 PQL 过程。


---
参考链接

* [配置数据源](/pql/properties.md)
* [PQL 基本语法](/pql/basic.md)
* [PQL 执行器 Worker](/worker/overview.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
