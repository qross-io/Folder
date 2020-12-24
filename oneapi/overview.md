# OneApi 概览
作者近年来一直从事的数据开发工作，第一次因为要做个后台接触Spring Boot的时候就被MyBatis整疯了，做一个接口有改6个文件！而且大部分的接口的核心都是一条SQL语句，无非就是简单的增删改查。虽然说有的代码可以自动生成，但是你做好的接口谁保证以后不改呀。于是作者就想办法做了一个简单的页面，用来管理这个后端项目的所有接口。再后来仅能写一条语句的输入框又添加了 IF 和 FOR 等控制结构。再后来有了[PQL](/pql/overview.md)，可以用 [PQL](/pql/overview.md) 来写接口逻辑了。这个项目最初的名字是 **ApiInOne**，现在叫 **OneApi**。因为有了 OneApi，部门没有招专职的 Java 开发工程师，即使最多时有3个前端工程师负责页面开发。基于 OneApi 开发的 Web 项目还可以为其他部门提供数据接口服务。

OneApi 有两种实现方式：一种是基于文件的接口管理方式，这些接口文件可以放在项目中，源代码受 **git** 管理，协作方便，[Master项目](/master/overview.md)使用这种方式; 另一种是基于数据库的接口管理方式，作者所在的部门就采用这种管理方式。作者正在把数据库管理方式在[Master项目](/master/overview.md)中重新开发。

无论哪种方式，接口逻辑都使用[PQL](/pql/overview.md)实现。虽说编写简单的接口不需要PQL的知识，但作者仍建议先[了解一下PQL](/pql/overview.md)。

基于文件的管理方式的接口示例（出自Master项目）：
```sql
## title | GET | id=1 |
SELECT title FROM jobs WHERE id=#{jobId} -> FIRST CELL

## title | PUT |
UPDATE qross_jobs SET title="#{"title"}", mender=@userid WHERE id=#{id}

## filter | GET | offset=0 |
IF $id IS NOT UNDEFINED THEN
    SELECT id, title, cron_exp FROM qross_jobs WHERE id=#{id};
ELSIF '#{title}' IS EMPTY THEN
    SELECT id, title, cron_exp FROM qross_jobs WHERE id<>#{jobId} ORDER BY id ASC LIMIT #{offset}, 15;
ELSE
    SELECT id, title, cron_exp FROM qross_jobs WHERE id<>#{jobId} AND title LIKE '%#{title}%' ORDER BY id ASC LIMIT #{offset}, 15;
END IF;
```

上例中实现了三个接口，这三个接口的设置都保存在文件`job.sql`中。除了你看到的代码，这两个接口不再需要编写其他的代码，不再像 SSM 模式下需要改那么多的文件。如接口地址可以是 `http://localhost:8080/api/job/title?id=2`，可以按需自定义。

OneApi可以极大的提高后端开发的效率，减少大量重复代码的开发工作。作者建议开发人员使用基于文件的接口管理方式，以方便自定义各种功能或者与其他的业务逻辑兼容; 数据开发人员可使用[Master项目](/master/overview.md)中的“接口”管理功能，不需要编写任何Java 代码。

OneApi 与 MyBatis 或 Hibernate 不同，不算是一个数据持久层框架，在使用这两个框架的项目中依然可以使用 OneApi。OneApi 打破了传统的 n 层架构的设计，没有 OOM 和 ORM 的概念。OneApi 目的并不是为了取代谁，而是提供一种在中小型项目快速开发接口和服务的解决方案。OneApi 的缺点可以总结为：

* 所有逻辑都使用 PQL 或 SQL 实现，所以要求开发人员有一定的 SQL 能力。
* 由 SQL 处理的业务流程因为过于简单而略过了太多细节，如果出错定位会比较困难。
* OneApi 没有自动缓存，需要自己通过 Redis 等方式实现。
* OneApi 可用于微服务等业务场景，可以与Spring Cloud集成。

---
参考链接

* [OneApi快速入门](/oneapi/quick.md)
* [OneApi设置](/oneapi/setup.md)