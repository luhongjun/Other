# gRPC 

## gRPC 是什么？
在 gRPC 里客户端应用可以像调用本地对象一样直接调用另一台不同的机器上服务端应用的方法，使得您能够更容易地创建分布式应用和服务。与许多 RPC 系统类似，gRPC 也是基于以下理念：定义一个服务，指定其能够被远程调用的方法（包含参数和返回类型）。在服务端实现这个接口，并运行一个 gRPC 服务器来处理客户端调用。在客户端拥有一个存根能够像服务端一样的方法。

![gRPC 客户端服务端交互图](gRPC%20客户端服务端交互图.png)

使用 gRPC 通信的大致流程如下：
1. 通过 `protocol buffers` 来定义通信双方（即客户端&服务端）的接口格式和数据类型；
2. 服务端根据 `protocol buffers` 文件实现接口逻辑，并开放接口及指定端口；
3. 客户端根据 `protocol buffers` 文件定义的接口，按需调用

## 使用 Protocol buffers
gRPC 默认使用 Protocol buffers，这是 Google 开源的一套成熟的结构数据序列化机制（当然也可以使用其他数据格式如 JSON）。

Protocol buffers 在 gitlab 的地址：https://github.com/protocolbuffers/protobuf ，我们可以看到官方定义的一些[内置的 proto 文件](https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf)

另外，关于 proto 的一些语法，也可以在[Protocol Buffer3 官方文档](https://developers.google.com/protocol-buffers/docs/proto3)参阅；

英文理解有障碍的话，可以参阅：
- [博客中文翻译 - Proto3语法指南](https://www.cnblogs.com/tohxyblog/p/8974763.html)
- [博客中文翻译 - Proto2语法指南](https://blog.csdn.net/qq_22660775/article/details/89044538)

*gRPC vs Restful API*

两者都提供了一套通信机制，用于server/client模型通信，而且它们都使用 HTTP 作为底层的传输协议(严格地说, gRPC 使用的 HTTP2.0，而 Restful API 则不一定)。不过gRPC还是有些特有的优势，如下：

- 需要对接口进行严格约束的情况，比如我们提供了一个公共的服务，很多人，甚至公司外部的人也可以访问这个服务，这时对于接口我们希望有更加严格的约束，我们不希望客户端给我们传递任意的数据，尤其是考虑到安全性的因素，我们通常需要对接口进行更加严格的约束。
- 对于性能有更高的要求时，可以通过 `Protocol Buffer` 我们可以将数据压缩编码转化为二进制格式（如果感兴趣的话可以深入了解[proto 编码原理](https://developers.google.com/protocol-buffers/docs/encoding)），通常传递的数据量要小得多，而且通过http2我们可以实现异步的请求，从而大大提高了通信效率。

## 实现 gRPC 通信 （Golang 实现示例）

详细可参阅[Protocol Buffer Basics: Go](https://developers.google.com/protocol-buffers/docs/gotutorial)

1. 双方协定并编写[`id-card.proto`文件](id_card.proto)

2. 服务端实现 gRPC 接口

- 2.1 服务端拿到这份`id-card.proto`文件后，需要借助 protoc 编译器转换成为相关的 go 文件以供调用

go proto 的编译器是`protoc`，可以通过以下命令安装：
``` 
go install google.golang.org/protobuf/cmd/protoc-gen-go
```

然后，我们使用[protoc 指令](Protoc(Protocol%20Compiler).md)对`api.proto`文件进行编译,编译后会得到一个文件`.pb.go`;





此外，还需要安装`protoc-gen-grpc-gateway`、`protoc-gen-swagger`：




