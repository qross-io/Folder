# 手动加载配置文件 LOAD

多数情况下，数据源都在配置文件或配置中心进行设置，在 Qross 系统中可以预先设置好这些[数据源配置](/pql/properties.md)，然后在编写 PQL 时可以直接调用。偶尔也需要使用一些不常用的配置，这些不常用的配置可以在使用前通过 PQL 的 **LOAD 语句**进行加载。

LOAD 语句支持三种格式的配置格式，分别是 Properties、YAML 文件和JSON。这些配置可以保存在文件中、Nacos 配置中心或其他 URL 地址中（其他配置中心）。

```sql
LOAD PROPERTIES '/config.properties';
LOAD YAML '/application.yml';
LOAD JSON CONFIG '/config.json';
LOAD PROPERTIES FROM NACOS  'localhost:8848:io.qross.test:connections';
LOAD YAML FROM URL  'http://www.domain.com/api/config';
```

* 上例中，从文件中加载配置先查找磁盘绝对路径下是否有这个文件，如果没有，则查找项目 resources 资源目录下是否有这个文件。
* Json 格式文件扩展名随意，不一定以`.json`结尾。
* Json 格式一定是一个 Object，可以像 Yaml 一样有多级。
* 从 Nacos 配置中心或 URL 地址中加载配置需要指定配置的格式。
* 从 Nacos 配置中心加载配置时，路径格式为 `host:port:group:data-id`，项之间使用冒号`:`分隔。
* 如果从 URL 地址中加载配置，这个 URL 可以是 Restful 风格的接口，也可以是其他配置中心的 HTTP 接口。


---
参考链接

* [PQL 数据源配置](/pql/properties.md)
