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

详细可参阅[Protocol Buffer Basics: Go](https://developers.google.com/protocol-buffers/docs/gotutorial)

1. 编写[`id-card.proto`文件](id_card.proto)

```
//需要以 proto3 的语法来解析此文件
syntax = "proto3";
//当前包名
package id_card
//声明 SearchRequest 消息体
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

2. 服务端实现 gRPC 接口

- 2.1 服务端拿到这份`id-card.proto`文件后，需要借助 protoc 编译器转换成为相关的 go 文件以供调用

go proto 的编译器是`protoc`，可以通过以下命令安装，此外，还需要安装`protoc`、`protoc-gen-grpc-gateway`、`protoc-gen-swagger`：
``` 
go install google.golang.org/protobuf/cmd/protoc-gen-go
```

然后，我们使用[protoc]()(详细也可以通过`protoc -help`来了解指令用法)对`api.proto`文件进行编译，执行：
``` 
protoc -I. -I./vendor -I./proto/ykproto  --grpc-gateway_out=logtostderr=true:. ./proto/id_card_proto/id_card/id_card.proto
```
--------------------
protoc 的用法具体如下：
``` 
root@SZ-PC-00517:/# protoc
Usage: protoc [OPTION] PROTO_FILES
Parse PROTO_FILES and generate output based on the options given:
  -IPATH, --proto_path=PATH   Specify the directory in which to search for
                              imports.  May be specified multiple times;
                              directories will be searched in order.  If not
                              given, the current working directory is used.
    指定要在其中搜索导入的目录。可以多次指定；将按顺序搜索目录。如果未给定，则使用当前工作目录。
  --version                   Show version info and exit.
  -h, --help                  Show this text and exit.
  --encode=MESSAGE_TYPE       Read a text-format message of the given type
                              from standard input and write it in binary
                              to standard output.  The message type must
                              be defined in PROTO_FILES or their imports.
  --decode=MESSAGE_TYPE       Read a binary message of the given type from
                              standard input and write it in text format
                              to standard output.  The message type must
                              be defined in PROTO_FILES or their imports.
  --decode_raw                Read an arbitrary protocol message from
                              standard input and write the raw tag/value
                              pairs in text format to standard output.  No
                              PROTO_FILES should be given when using this
                              flag.
  --descriptor_set_in=FILES   Specifies a delimited list of FILES
                              each containing a FileDescriptorSet (a
                              protocol buffer defined in descriptor.proto).
                              The FileDescriptor for each of the PROTO_FILES
                              provided will be loaded from these
                              FileDescriptorSets. If a FileDescriptor
                              appears multiple times, the first occurrence
                              will be used.
  -oFILE,                     Writes a FileDescriptorSet (a protocol buffer,
    --descriptor_set_out=FILE defined in descriptor.proto) containing all of
                              the input files to FILE.
  --include_imports           When using --descriptor_set_out, also include
                              all dependencies of the input files in the
                              set, so that the set is self-contained.
  --include_source_info       When using --descriptor_set_out, do not strip
                              SourceCodeInfo from the FileDescriptorProto.
                              This results in vastly larger descriptors that
                              include information about the original
                              location of each decl in the source file as
                              well as surrounding comments.
  --dependency_out=FILE       Write a dependency output file in the format
                              expected by make. This writes the transitive
                              set of input file paths to FILE
  --error_format=FORMAT       Set the format in which to print errors.
                              FORMAT may be 'gcc' (the default) or 'msvs'
                              (Microsoft Visual Studio format).
  --print_free_field_numbers  Print the free field numbers of the messages
                              defined in the given proto files. Groups share
                              the same field number space with the parent
                              message. Extension ranges are counted as
                              occupied fields numbers.

  --plugin=EXECUTABLE         Specifies a plugin executable to use.
                              Normally, protoc searches the PATH for
                              plugins, but you may specify additional
                              executables not in the path using this flag.
                              Additionally, EXECUTABLE may be of the form
                              NAME=PATH, in which case the given plugin name
                              is mapped to the given executable even if
                              the executable's own name differs.
  --cpp_out=OUT_DIR           Generate C++ header and source.
  --csharp_out=OUT_DIR        Generate C# source file.
  --java_out=OUT_DIR          Generate Java source file.
  --js_out=OUT_DIR            Generate JavaScript source.
  --objc_out=OUT_DIR          Generate Objective C header and source.
  --php_out=OUT_DIR           Generate PHP source file.
  --python_out=OUT_DIR        Generate Python source file.
  --ruby_out=OUT_DIR          Generate Ruby source file.
  @<filename>                 Read options and filenames from file. If a
                              relative file path is specified, the file
                              will be searched in the working directory.
                              The --proto_path option will not affect how
                              this argument file is searched. Content of
                              the file will be expanded in the position of
                              @<filename> as in the argument list. Note
                              that shell expansion is not applied to the
                              content of the file (i.e., you cannot use
                              quotes, wildcards, escapes, commands, etc.).
                              Each line corresponds to a single argument,
                              even if it contains spaces.
```
--------------------


等`id-card.proto`文件编译完后，默认地（如果不指定 OUT_DIR），会在当前目录下生成三个文件：

a. `id_card.pb.go`文件

值得关注的是，当前 go 文件的包名即是`id-card.proto`文件的包声明，还有两个比较重要的函数：
``` 
//此函数是给【服务端】调用的，注册gRPC的服务器。
//- 函数第一入参是依赖于外部官方的"gRPC.Server"（链接：https://pkg.go.dev/google.golang.org/grpc?readme=expanded#Server）。
//
func RegisterIDCardServiceServer(s *grpc.Server, srv IDCardServiceServer) {
	s.RegisterService(&_IDCardService_serviceDesc, srv)
}
//此方法是给【客户端】调用的，生成gRPC的客户端连接，它的入参依赖于"gRPC库-ClientConn"（链接：https://pkg.go.dev/google.golang.org/grpc?readme=expanded#ClientConn）
//
func NewIDCardServiceClient(cc *grpc.ClientConn) IDCardServiceClient {
	return &iDCardServiceClient{cc}
}



```
    
b. `id_card.pb.gw.go`文件：此文件是 gateway 文件
c. `id_card.swagger.json`文件：生成 swagger




