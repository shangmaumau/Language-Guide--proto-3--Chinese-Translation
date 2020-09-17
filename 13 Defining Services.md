13 Defining Services.md

Defining Services

If you want to use your message types with an RPC (Remote Procedure Call) system, you can define an RPC service interface in a .proto file and the protocol buffer compiler will generate service interface code and stubs in your chosen language. So, for example, if you want to define an RPC service with a method that takes your SearchRequest and returns a SearchResponse, you can define it in your .proto file as follows:

service SearchService {
  rpc Search(SearchRequest) returns (SearchResponse);
}

The most straightforward RPC system to use with protocol buffers is gRPC: a language- and platform-neutral open source RPC system developed at Google. gRPC works particularly well with protocol buffers and lets you generate the relevant RPC code directly from your .proto files using a special protocol buffer compiler plugin.

If you don't want to use gRPC, it's also possible to use protocol buffers with your own RPC implementation. You can find out more about this in the Proto2 Language Guide.

There are also a number of ongoing third-party projects to develop RPC implementations for Protocol Buffers. For a list of links to projects we know about, see the third-party add-ons wiki page.
