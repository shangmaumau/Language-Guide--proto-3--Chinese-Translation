
# Defining Services

# 定义服务

If you want to use your message types with an RPC (Remote Procedure Call) system, you can define an RPC service interface in a .proto file and the protocol buffer compiler will generate service interface code and stubs in your chosen language. So, for example, if you want to define an RPC service with a method that takes your SearchRequest and returns a SearchResponse, you can define it in your .proto file as follows:

如果你想与一个 RPC（远程过程调用）一起使用你的消息类型，你可以在一个 `.proto` 文件中定义一个 RPC 服务接口，然后 protocol buffer 编译器就会生成服务接口代码并以你选择的语言插桩（stub）。那么，举个例子，如果你想定义一个 RPC 服务——一个方法，拿着你的 `SearchRequest` 然后返回一个 `SearchResponse`，你可以像下面这样在你的 `.proto` 文件中定义它：

```proto
service SearchService {
  rpc Search(SearchRequest) returns (SearchResponse);
}
```

The most straightforward RPC system to use with protocol buffers is gRPC: a language- and platform-neutral open source RPC system developed at Google. gRPC works particularly well with protocol buffers and lets you generate the relevant RPC code directly from your .proto files using a special protocol buffer compiler plugin.

与 protocol buffers 一起使用的最简单直接的 RPC 系统是 [gRPC](https://grpc.io/) —— 由谷歌开发的一个语言和平台中立的开源 RPC 系统。gRPC 与 protocol buffers 工作得尤其好，并使用一个特殊的 protocol buffer 编译器插件让你由你的 .proto 文件直接生成相关 RPC 代码。

If you don't want to use gRPC, it's also possible to use protocol buffers with your own RPC implementation. You can find out more about this in the Proto2 Language Guide.

如果你不想使用 gRPC，用你自己的 RPC 实现来使用 protocol buffers 也是可以的。关于这点，你可以在 [Proto2 语言指南](https://developers.google.com/protocol-buffers/docs/proto#services)中查看到更多信息。

There are also a number of ongoing third-party projects to develop RPC implementations for Protocol Buffers. For a list of links to projects we know about, see the third-party add-ons wiki page.

也有若干正在进行中的第三方项目在为 Protocol Buffers 开发 RPC 实现。（如果你）想要一个我们了解的这些项目的链接列表，请查看[第三方插件维基页面](https://github.com/protocolbuffers/protobuf/blob/master/docs/third_party.md)。
