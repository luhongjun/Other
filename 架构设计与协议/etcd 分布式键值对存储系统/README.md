# etcd

etcd 是 CoreOS 团队于2013年6月发起的开源项目，它的目标是构建一个高可用的*分布式*键值(key-value)数据库。

etcd 内部采用 raft 协议为一致性算法。通过分布式锁，leader 选举和写屏障(write barriers)来实现可靠的分布式协作。etcd 集群是为高可用，持久性数据存储和检索而准备。

[etcd](https://github.com/etcd-io/etcd)基于 Go 语言实现。

这里有[etcd 中文版文档](https://doczhcn.gitbook.io/etcd/)

## etcd 应用场景

etcd 比较多的应用场景是用于服务发现，服务发现(Service Discovery)要解决的是分布式系统中最常见的问题之一，即在同一个分布式集群中的进程或服务如何才能找到对方并建立连接。

从本质上说，服务发现就是要了解集群中是否有进程在监听 UDP 或者 TCP 端口，并且通过名字就可以进行查找和链接。

要解决服务发现的问题，需要下面三大支柱，缺一不可：

- 强一致性、高可用的服务存储目录

基于[Raft算法](https://www.cnblogs.com/xybaby/p/10124083.html)的 etcd 天生就是这样一个强一致性、高可用的服务存储目录;

- 注册服务和健康服务健康状况的机制

用户可以在 etcd 中注册服务，并且对注册的服务配置 key TTL，定时保持服务的心跳以达到监控健康状态的效果;

- 查找和连接服务的机制

通过在 etcd 指定的主题下注册的服务也能在对应的主题下查找到。为了确保连接，我们可以在每个服务机器上都部署一个 proxy 模式的 etcd，这样就可以确保访问 etcd 集群的服务都能够互相连接

## 安装

- [etcd 安装教程](https://etcd.io/docs/v3.4/dl-build/)

## 使用

- [etcd 命令行参数说明](etcd%20命令行参数说明.txt)

- [etcdctl 命令行参数说明](etcdctl%20命令行参数说明.txt)

etcdctl 是一个和 etcd 服务器交互的命令行工具。这里描述的概念也适用于 gRPC API 或者客户端类库 API。






etcd 在键的组织上采用了层次化的空间结构(类似于文件系统中目录的概念)，用户指定的键可以为单独的名字，如:testkey，此时实际上放在根目录/下面，也可以为指定目录结构，如/cluster1/node2/testkey，则将创建相应的目录结构。

``` 

```