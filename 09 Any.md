
# Any

The Any message type lets you use messages as embedded types without having their .proto definition. An Any contains an arbitrary serialized message as bytes, along with a URL that acts as a globally unique identifier for and resolves to that message's type. To use the Any type, you need to import google/protobuf/any.proto.

`Any` 消息类型让你可以在不拥有消息的 .proto 定义的情况下，将其作为嵌入类型使用。一个 Any 包含了一个以 `bytes`（呈现的）任意序列化的消息，还有一个 URL，充当一个全局唯一标识符，来代表并决定那个消息的类型。要使用 `Any`类型，你需要[导入](https://developers.google.com/protocol-buffers/docs/proto3#other) `google/protobuf/any.proto`。<sup>[注]</sup>

【**注**在 3.12.0 版本中，原有的 `any.proto` 已废弃，请使用相应语言目录下的 `GPBAny.proto` 来替代。】

```proto
import "google/protobuf/any.proto";

message ErrorStatus {
  string message = 1;
  repeated google.protobuf.Any details = 2;
}
```

The default type URL for a given message type is `type.googleapis.com/_packagename_._messagename_`.

一个给定的消息类型的默认类型 URL 是 `type.goolgeapis.com/_packagename_._messagename_`。

Different language implementations will support runtime library helpers to pack and unpack Any values in a typesafe manner – for example, in Java, the Any type will have special pack() and unpack() accessors, while in C++ there are PackFrom() and UnpackTo() methods:

不同语言的实现会支持运行时库协助者（runtime library helper）用一种类型安全的方式来打包和解包 Any 值——举例来说，在 Java 中，Any 类型会有一个特殊的 `pack()` 和 `unpack()` 访问器，而在 C++ 中则是 `PackFrom()` 和 `UnpackTo()` 方法：


```cpp
// Storing an arbitrary message type in Any.
// 以 Any 存储一条任意消息类型

NetworkErrorDetails details = ...;
ErrorStatus status;
status.add_details()->PackFrom(details);

// Reading an arbitrary message from Any.
// 从 Any 读取一条任意消息

ErrorStatus status = ...;
for (const Any& detail : status.details()) {
  if (detail.Is<NetworkErrorDetails>()) {
    NetworkErrorDetails network_error;
    detail.UnpackTo(&network_error);
    ... processing network_error ...
  }
}
```

Currently the runtime libraries for working with Any types are under development.

**目前支持与 Any 类型一同工作的运行时库仍在开发中。**

If you are already familiar with proto2 syntax, the Any can hold arbitrary proto3 messages, similar to proto2 messages which can allow extensions.

如果你已经熟悉 [proto2 句法](https://developers.google.com/protocol-buffers/docs/proto)，`Any` 可保存（hold）任意 proto3 消息，类似于 proto2 消息可以允许 [extensions](https://developers.google.com/protocol-buffers/docs/proto#extensions)。