# Apollo 配置中心

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

服务端基于Spring Boot和Spring Cloud开发，打包后可以直接运行，不需要额外安装Tomcat等应用容器。

Java客户端不依赖任何框架，能够运行于所有Java运行时环境，同时对Spring/Spring Boot环境也有较好的支持。

.Net客户端不依赖任何框架，能够运行于所有.Net运行时环境。

---------------
Apollo 整体架构如下图：
![Apollo架构图](Apollo架构图.png)

上图简要描述了Apollo的总体设计，我们可以从下往上看：

- Config Service 提供配置的读取、推送等功能，服务对象是 Apollo 客户端
- Admin Service 提供配置的修改、发布等功能，服务对象是 Apollo Portal（管理界面）
- Config Service 和 Admin Service 都是多实例、无状态部署，所以需要将自己注册到 Eureka 中并保持心跳
- 在 Eureka 之上我们架了一层 Meta Server 用于封装 Eureka 的服务发现接口
- Client 通过域名访问 Meta Server 获取 Config Service 服务列表（IP+Port），而后直接通过IP+Port访问服务，同时在 Client 则会做 load balance、错误重试
- Portal 通过域名访问 Meta Server 获取 Admin Service 服务列表（IP+Port），而后直接通过IP+Port访问服务，同时在Portal 则会做 load balance、错误重试
- 为了简化部署，我们实际上会把 Config Service、Eureka 和 Meta Server 三个逻辑角色部署在同一个JVM进程中

参考：[微服务架构~携程Apollo配置中心架构剖析](https://mp.weixin.qq.com/s/-hUaQPzfsl9Lm3IqQW3VDQ)
---------------

更多详细资料请参阅[Apollo 中文文档](https://ctripcorp.github.io/apollo/#/zh/README)

## 核心概念

我们有必要先来了解一下Apollo中的几个核心概念：

- application (应用)

这个很好理解，就是实际使用配置的应用，Apollo客户端在运行时需要知道当前应用是谁，从而可以去获取对应的配置。

每个应用都需要有唯一的身份标识 -- appId，我们认为应用身份是跟着代码走的。

- environment (环境)

配置对应的环境，Apollo客户端在运行时需要知道当前应用处于哪个环境，从而可以去获取应用的配置。

我们认为环境和代码无关，同一份代码部署在不同的环境就应该能够获取到不同环境的配置。

环境默认是通过读取机器上的配置（server.properties中的env属性）指定的

- cluster (集群)

一个应用下不同实例的分组，比如典型的可以按照数据中心分，把上海机房的应用实例分为一个集群，把北京机房的应用实例分为另一个集群；

对不同的cluster，同一个配置可以有不一样的值，如zookeeper地址；

集群默认是通过读取机器上的配置（server.properties中的idc属性）指定的；

- [namespace (命名空间)](https://github.com/ctripcorp/apollo/wiki/Apollo%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5%E4%B9%8B%E2%80%9CNamespace%E2%80%9D)

一个应用下不同配置的分组，可以简单地把 namespace 类比为文件，不同类型的配置存放在不同的文件中，如数据库配置文件，RPC配置文件，应用自身的配置文件等

应用可以直接读取到公共组件的配置namespace，如DAL，RPC等；

应用也可以通过继承公共组件的配置namespace来对公共组件的配置做调整，如DAL的初始数据库连接数；