# gRPC 

## gRPC 是什么？
在 gRPC 里客户端应用可以像调用本地对象一样直接调用另一台不同的机器上服务端应用的方法，使得您能够更容易地创建分布式应用和服务。与许多 RPC 系统类似，gRPC 也是基于以下理念：定义一个服务，指定其能够被远程调用的方法（包含参数和返回类型）。在服务端实现这个接口，并运行一个 gRPC 服务器来处理客户端调用。在客户端拥有一个存根能够像服务端一样的方法。

使用 gRPC 通信的大致流程如下：
1. 通过 `protocol buffers` 来定义通信双方（即客户端&服务端）的接口格式和数据类型；
2. 服务端根据 `protocol buffers` 文件实现接口逻辑，并开放接口及指定端口；
3. 客户端根据 `protocol buffers` 文件按需调用

## 使用 Protocol buffers
gRPC 默认使用 Protocol buffers，这是 Google 开源的一套成熟的结构数据序列化机制（当然也可以使用其他数据格式如 JSON）。

可以在[Protocol Buffer3 官方文档](https://developers.google.com/protocol-buffers/docs/proto3)参阅更多`.proto`文件的语法。

*gRPC vs Restful API*

两者都提供了一套通信机制，用于server/client模型通信，而且它们都使用 HTTP 作为底层的传输协议(严格地说, gRPC 使用的 HTTP2.0，而 Restful API 则不一定)。不过gRPC还是有些特有的优势，如下：

- 需要对接口进行严格约束的情况，比如我们提供了一个公共的服务，很多人，甚至公司外部的人也可以访问这个服务，这时对于接口我们希望有更加严格的约束，我们不希望客户端给我们传递任意的数据，尤其是考虑到安全性的因素，我们通常需要对接口进行更加严格的约束。
- 对于性能有更高的要求时，可以通过 `Protocol Buffer` 我们可以将数据压缩编码转化为二进制格式（如果感兴趣的话可以深入了解[proto 编码原理](https://developers.google.com/protocol-buffers/docs/encoding)），通常传递的数据量要小得多，而且通过http2我们可以实现异步的请求，从而大大提高了通信效率。

## 实现 gRPC 通信 （Golang 实现示例）

1. 编写 `*.proto` 文件

```api.proto
// 需要以 proto3 的语法来解析此文件
syntax = "proto3";
// 当前包名
package luhj_service

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```


