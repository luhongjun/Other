# RabbitMQ

相关资料查阅：

- [RabbitMQ 中文文档](http://rabbitmq.mr-ping.com/)
- [AMQP 0-9-1 Quick Reference](https://www.rabbitmq.com/amqp-0-9-1-quickref.html)
  
AMQP（高级消息队列协议）是一个网络协议。它支持符合要求的客户端应用（application）和消息中间件代理（messaging middleware broker）之间进行通信

- [高级消息队列协议 - AMQP(Advanced Message Queuing Protocol)](https://www.amqp.org/)
- [Go RabbitMQ Client Library](https://pkg.go.dev/github.com/streadway/amqp?utm_source=godoc)

## 概述
RabbitMQ是一个消息代理 - 一个消息系统的媒介。它可以为你的应用提供一个通用的消息发送和接收平台，并且保证消息在传输过程中的安全。

| 技术亮点 | 说明 |
| --- | --- |
| 可靠性 | RabbitMQ提供了多种技术可以让你在性能和可靠性之间进行权衡。这些技术包括持久性机制、投递确认、发布者证实和高可用性机制 |
| 灵活的路由 | 消息在到达队列前是通过交换机进行路由的。RabbitMQ为典型的路由逻辑提供了多种内置交换机类型。如果你有更复杂的路由需求，可以将这些交换机组合起来使用，你甚至可以实现自己的交换机类型，并且当做RabbitMQ的插件来使用 |
| 集群 | 在相同局域网中的多个RabbitMQ服务器可以聚合在一起，作为一个独立的逻辑代理来使用 |
| 联合 | 对于服务器来说，它比集群需要更多的松散和非可靠链接。为此RabbitMQ提供了联合模型 |
| 高可用的队列 | 在同一个集群里，队列可以被镜像到多个机器中，以确保当其中某些硬件出现故障后，你的消息仍然安全 |
| 多协议 | RabbitMQ 支持多种消息协议的消息传递 |

## 安装

安装教程可参考官网 [Downloading and Installing RabbitMQ](https://www.rabbitmq.com/download.html)

可以通过 [RabbitMQ 镜像](https://hub.docker.com/_/rabbitmq?tab=tags&page=1&ordering=last_updated)的方式在 Docker 中下载安装：
```shell script
# 直接拉取最新的 lastest 的镜像
docker pull rabbitmq
# for RabbitMQ 3.8
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

## Rabbitmq 管理工具 
使用命令 rabbitmqctl 来与rabbitmq服务器交互

- [官网 - 命令行大全](https://www.rabbitmq.com/rabbitmqctl.8.html)
- [简书 - RabbitMQ手册之rabbitmqctl](https://www.jianshu.com/p/61a90fba1d2a)

### 单节点的rabbitmq管理
//@todo
### 集群的rabbitmq管理
//@todo
### 复制
//@todo
### 用户管理
//@todo
### 权限管理
//@todo
### 虚拟主机 Virtual hosts
//@todo








监控工具 rabbitmq-diagnostics：
插件管理 rabbitmq-plugins：https://www.rabbitmq.com/rabbitmq-plugins.8.html
队列管理工具 rabbitmq-queues：https://www.rabbitmq.com/rabbitmq-queues.8.html
HTTP API管理 rabbitmqadmin：https://www.rabbitmq.com/management-cli.html
https://www.rabbitmq.com/manpages.html