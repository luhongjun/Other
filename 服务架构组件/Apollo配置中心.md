# Apollo 配置中心

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

服务端基于Spring Boot和Spring Cloud开发，打包后可以直接运行，不需要额外安装Tomcat等应用容器。

## 资料查阅

- [微服务架构~携程Apollo配置中心架构剖析](https://mp.weixin.qq.com/s/-hUaQPzfsl9Lm3IqQW3VDQ)
- [Apollo 官方文档](https://ctripcorp.github.io/apollo/#/zh/)
- [GitHub - ctripcorp/apollo, wiki](https://github.com/ctripcorp/apollo/wiki)


## Apollo 接入指南

- [Apollo开放平台接入指南](https://ctripcorp.github.io/apollo/#/zh/usage/apollo-open-api-platform)

Apollo提供了一套的Http REST接口，使第三方应用能够自己管理配置。虽然Apollo系统本身提供了Portal来管理配置，但是在有些情景下，应用需要通过程序去管理配置。
  
- [PHP 客户端使用指南](https://ctripcorp.github.io/apollo/#/zh/usage/third-party-sdks-user-guide?id=_4-php)
- [Golang 客户端使用指南](https://ctripcorp.github.io/apollo/#/zh/usage/third-party-sdks-user-guide?id=_1-go)


