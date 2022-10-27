# Other

记录开发运维、架构思想等非语言类知识

## 知识点导航
- Linux系统与运维
- 服务架构
    - 网关Gateway
        - [Apache APISIX](服务架构/网关Gateway/Apache%20APISIX/README.md)
        - [Kong]()
        - [Nginx]()
    - 容器
        - [Docker]()
        - [Kubernetes](服务架构/容器/Kubernetes/README.md)：用于自动部署，扩展和管理容器化应用程序的开源系统
            - [《Kubernetes进阶训练营.doc》](服务架构/容器/Kubernetes/Kubernetes进阶.docx)（原文教程：https://www.qikqiak.com/k8strain2/）
    - 数据库
        - [Apollo](服务架构/数据库/Apollo配置中心/README.md)：携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景
          - [>> Apollo中文文档](https://www.apolloconfig.com/#/zh/README) 
          - [《Apollo配置中心架构剖析.docx》](服务架构/数据库/Apollo配置中心/Apollo配置中心架构剖析.docx)(原文链接：https://mp.weixin.qq.com/s/-hUaQPzfsl9Lm3IqQW3VDQ)
        - [etcd](服务架构/数据库/etcd分布式键值对存储系统/README.md)：它的目标是构建一个高可用的分布式键值(key-value)数据库
          - [>> etcd官方文档（英文）](https://etcd.io/docs/)
          - [>> etcd官方文档（中文翻译）](https://doczhcn.gitbook.io/etcd/)
        - [MySQL]()
        - [Redis]()
        - [MongoDB]()
        - [elsearch]()
    - 服务注册与发现
        - [Eureka]()
    - 服务监控与运维
        - [Prometheus](服务架构/服务监控与运维/Promethues/README.md)：是由前 Google 工程师从 2012 年开始在 Soundcloud 以开源软件的形式进行研发的系统监控和告警工具包，自此以后，许多公司和组织都采用了 Prometheus 作为监控告警工具
            - [>> Prometheus官方文档（英文）](https://prometheus.io/docs)
            - [>> Prometheus文档（中文翻译）](https://prometheus.fuckcloudnative.io/)
            - [Golang开发：Prometheus Go client library](https://pkg.go.dev/github.com/prometheus/client_golang)
            - [《Prometheus监控系统快速入门与实践[极客时间]-马永亮.pdf》](服务架构/服务监控与运维/Promethues/Prometheus监控系统快速入门与实践%5B极客时间%5D-马永亮.pdf)
            - [《Prometheus内部原理.docx》](服务架构/服务监控与运维/Promethues/Prometheus内部原理.docx)
        - Thanos：简化分布式 Prometheus 的部署与管理，并提供了一些的高级特性：全局视图，长期存储，高可用
            - [《打造云原生大型分布式监控系统-Thanos.docx》](服务架构/服务监控与运维/Thanos/打造云原生大型分布式监控系统-Thanos.docx)
            - [>> thanos官网文档（英文）](https://thanos.io/v0.29/thanos/getting-started.md/)
    - 消息中间件
        - [Kafka](服务架构/消息中间件/Kafka/README.md)：一个拥有高吞吐、可持久化、可水平扩展，支持流式数据处理等多种特性的分布式消息流处理中间件，采用分布式消息发布与订阅机制，在日志收集、流式数据传输、在线/离线系统分析、实时监控等领域有广泛的应用
        - RabbitMQ
        - ZeroMQ
- 设计模式
    - [实现领域驱动设计_Vaughn Vernon著.pdf](设计模式/实现领域驱动设计_Vaughn%20Vernon著.pdf)