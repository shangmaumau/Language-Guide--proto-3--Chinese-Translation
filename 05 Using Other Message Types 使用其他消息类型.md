05 Using Other Message Types

# Using Other Message Types

# 使用其他消息类型

You can use other message types as field types. For example, let's say you wanted to include Result messages in each SearchResponse message – to do this, you can define a Result message type in the same .proto and then specify a field of type Result in SearchResponse:

你可以使用其他消息类型来作为字段类型。举个例子，比方说你想在每个 `SearchResponse` 消息中包含 `Result` 消息，要做到这点，你可以在同一个 `.proto` 中定义一个 `Result` 消息类型，然后在 `SearchResponse` 中指定一个类型为 `Result` 的字段：

```proto
message SearchResponse {
  repeated Result results = 1;
}

message Result {
  string url = 1;
  string title = 2;
  repeated string snippets = 3;
}

```

## Importing Definitions

## 导入定义

In the above example, the Result message type is defined in the same file as SearchResponse – what if the message type you want to use as a field type is already defined in another .proto file?

在上面的示例中，`Result` 消息类型和 `SearchResponse` 定义在了同一个文件中——如果你想要用来作为字段类型的消息类型已经在其他 `.proto` 文件中定义过了，该怎么办呢？

You can use definitions from other .proto files by importing them. To import another .proto's definitions, you add an import statement to the top of your file:

你可能通过*导入*来使用其他 `.proto` 文件中的（消息类型）定义。要导入另一个 `.proto` 的定义，你需要在你的文件的顶部添加一条 import 语句：

```proto
import "myproject/other_protos.proto";
```

By default you can only use definitions from directly imported .proto files. However, sometimes you may need to move a .proto file to a new location. Instead of moving the .proto file directly and updating all the call sites in a single change, now you can put a dummy .proto file in the old location to forward all the imports to the new location using the import public notion. import public dependencies can be transitively relied upon by anyone importing the proto containing the import public statement. For example:

默认情况下，你只能使用来自直接导入的 `.proto` 文件中的定义。但是，有时候你可能需要把一个 `.proto` 文件移动到一个新位置。与直接移动 `.proto` 文件，然后一口气（in a single change）更新所有调用的地方不同的是，现在你可以使用 `import public` 的概念，来在老位置放一个傀儡<sup>[注]</sup> `.proto` 文件，以转递所有的 imports 到一个新位置。无论是谁导入了包含着 `import public` 语句的 proto，都可以传递性地依赖（transitively relied upon） `import public` 依赖项。

（**注** `dummy` 更多采用的是“**虚设**”这样的译法，但我认为，虚设并不能准确表达这里的 dummy 的意思。虚设的意思是空有形式，并不起作用。但这里的 .proto 文件不要它能行吗？不行（无法转递了）。所以其实是有用的，就是作为一个傀儡，而背后有真正的操作者通过它来传达指令和接收请求。文中讲到的这个 .proto 文件就是这样的用途，傀儡，其实是非常准确传达了这个意味。）

```proto
// new.proto
// All definitions are moved here
// 所有定义都移到了这里
```

```proto
// old.proto
// This is the proto that all clients are importing.
// 这是所有的客户都导入了的 proto
import public "new.proto";
import "other.proto";
```

```proto
// client.proto
import "old.proto";
// You use definitions from old.proto and new.proto, but not other.proto
// 你可以使用来自 old.proto 和 new.proto 的消息定义，但 other.proto 中的则不行 
```

The protocol compiler searches for imported files in a set of directories specified on the protocol compiler command line using the -I/--proto_path flag. If no flag was given, it looks in the directory in which the compiler was invoked. In general you should set the --proto_path flag to the root of your project and use fully qualified names for all imports.

protocol 编译器在一组由 protocol 编译器命令行使用 `-I` 或 `--proto_path` 标记指定的文件目录中搜索导入的文件。如果没有给定标记，它就在调起编译器的那个目录里寻找。通常而言，你应该把 `--proto_path` 标记设置到你的工程的根目录，并为所有的 imports 使用[完全限定名称](https://en.wikipedia.org/wiki/Fully_qualified_name)。

## Using proto2 Message Types

## 使用 proto2 消息类型

It's possible to import proto2 message types and use them in your proto3 messages, and vice versa. However, proto2 enums cannot be used directly in proto3 syntax (it's okay if an imported proto2 message uses them).

在你的 proto3 消息中导入并使用 [proto2](https://developers.google.com/protocol-buffers/docs/proto) 消息类型，是可以的，反过来也行。但是，proto2 枚举不能直接用在 proto3 句法里（如果一个导入了的 proto2 消息使用了它们，则没有问题）。


