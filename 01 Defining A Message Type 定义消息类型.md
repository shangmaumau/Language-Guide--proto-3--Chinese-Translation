
# Defining A Message Type

# 定义消息类型

First let's look at a very simple example. Let's say you want to define a search request message format, where each search request has a query string, the particular page of results you are interested in, and a number of results per page. Here's the .proto file you use to define the message type.

首先让我们看一个非常简单的例子。比方说你想要定义一个搜索请求的消息形式，每个搜索请求都有一个查询字符串，你感兴趣的结果所在的特定的页，以及每页多少个结果的一个数字。这里就是你用来定义消息类型的 .proto 文件。

```proto
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

- The first line of the file specifies that you're using proto3 syntax: if you don't do this the protocol buffer compiler will assume you are using proto2. This must be the first non-empty, non-comment line of the file.

- 文件中第一行指明了你正在使用 proto3 的句法：如果你不这么做，protocol buffer 编译器就会假定你正在使用 [proto2](https://developers.google.com/protocol-buffers/docs/proto)。文件的第一行，必须是这样的——既不能为空，也不能注释掉。

- The SearchRequest message definition specifies three fields (name/value pairs), one for each piece of data that you want to include in this type of message. Each field has a name and a type.

- SearchRequest 的消息定义指定了三个字段（名称/数值对儿），每一块你想要包含在此消息类型中的数据，都各有一个字段。每个字段都有一个名称和一个类型。

## Specifying Field Types

## 指定字段类型

In the above example, all the fields are scalar types: two integers (page_number and result_per_page) and a string (query). However, you can also specify composite types for your fields, including enumerations and other message types.

在上面的例子中，所有字段都是[标量类型](https://developers.google.com/protocol-buffers/docs/proto3#scalar)：两个整数（page_number 和 result_per_page），以及一个字符串（query）。但是，你也可以为你的字段指定复合类型，包括[枚举](https://developers.google.com/protocol-buffers/docs/proto3#enum)和其他消息类型。

## Assigning Field Numbers

## 分配字段编号

As you can see, each field in the message definition has a unique number. These field numbers are used to identify your fields in the message binary format, and should not be changed once your message type is in use. Note that field numbers in the range 1 through 15 take one byte to encode, including the field number and the field's type (you can find out more about this in Protocol Buffer Encoding). Field numbers in the range 16 through 2047 take two bytes. So you should reserve the numbers 1 through 15 for very frequently occurring message elements. Remember to leave some room for frequently occurring elements that might be added in the future.

正如你看到的那样，消息定义中的每个字段都有一个唯一的编号。这些字段编号是用来在[消息体的二进制形式](https://developers.google.com/protocol-buffers/docs/encoding)中识别你的字段的，一旦你的消息类型使用起来了，这些编号就不该再改变。请注意，字段编号在 1-15 之间的，使用一个字节来编码，包括字段编号和字段的类型（有关这一点，在 [Protocol Buffer 编码](https://developers.google.com/protocol-buffers/docs/encoding#structure)中，你能查看到更多信息）。字段编号在 16-2047 之间的则使用两个字节。因此你应当为非常频繁出现的消息元素保留 1-15 的编号。记得给未来可能添加进来的、频繁出现的消息元素保留一些空间。

The smallest field number you can specify is 1, and the largest is $2^{29}$ - 1, or 536,870,911. You also cannot use the numbers 19000 through 19999 (FieldDescriptor::kFirstReservedNumber through FieldDescriptor::kLastReservedNumber), as they are reserved for the Protocol Buffers implementation - the protocol buffer compiler will complain if you use one of these reserved numbers in your .proto. Similarly, you cannot use any previously reserved field numbers.

你能指定的最小的字段编号是 1，最大的是 $2^{29}$ - 1，或 536,870,911。你也不能使用 19000-19999 之间的编号（`FieldDescriptor::kFirstReservedNumber` 到 `FieldDescriptor::kLastReservedNumber`），因为它们被保留用于 Protocol Buffers 的实现——如果你在你的 .proto 文件中使用了某一个这些保留的编号，protocol buffer 编译器就会报错。同样地，你不能使用任何之前已经保留的字段编号。


## Specifying Field Rules

## 指定字段的规则

Message fields can be one of the following:

消息字段可以是下面所列中的某一类：

- singular: a well-formed message can have zero or one of this field (but not more than one). And this is the default field rule for proto3 syntax.
- singular：格式正确的消息可以不需要此字段，或拥有一个此字段（但同样的字段不能超过一个）。这是 proto3 句法的默认字段规则。
- `repeated`: this field can be repeated any number of times (including zero) in a well-formed message. The order of the repeated values will be preserved.
- `repeated`：在格式正确的消息中，此字段可重复任意次数（包括〇次）。重复的值的序列会被保持。

In proto3, repeated fields of scalar numeric types use packed encoding by default.

在 proto3 中，标量数值类型的 `repeated` 字段默认使用`分组`编码（`packed` encoding）。

You can find out more about `packed` encoding in Protocol Buffer Encoding.

有关`分组`编码，你可以在 [Protocol Buffer 编码](https://developers.google.com/protocol-buffers/docs/encoding#packed) 中查找到更多信息。

## Adding More Message Types

## 添加更多消息类型

Multiple message types can be defined in a single .proto file. This is useful if you are defining multiple related messages – so, for example, if you wanted to define the reply message format that corresponds to your SearchResponse message type, you could add it to the same .proto:

在单个的 `.proto` 文件中，可以定义多个消息类型。如果你正在定义多个相关联的消息，这就有用了——那么，举例来说，如果你想要定义与你的 `SearchRequest` 消息类型对应的回复消息形式，你可以将它添加进同一个 `.proto` 文件中：

```proto
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}

message SearchResponse {
 ...
}
```

## Adding Comments

## 添加注释

To add comments to your .proto files, use C/C++-style // and /* ... */ syntax.

如果要往你的 `.proto` 文件中添加注释，请使用 C/C++ 风格的 `//` 和 `/* ... */` 句法。

```proto

/* SearchRequest represents a search query, with pagination options to
 * indicate which results to include in the response. */

/* SearchRequest 表示一个搜索请求，附带有标页码选项，
 * 来标示在响应中要包含哪些结果。*/

message SearchRequest {
  string query = 1;
  int32 page_number = 2;  // Which page number do we want?
  int32 result_per_page = 3;  // Number of results to return per page.
}

```

## Reserved Fields

## 保留字段

If you update a message type by entirely removing a field, or commenting it out, future users can reuse the field number when making their own updates to the type. This can cause severe issues if they later load old versions of the same .proto, including data corruption, privacy bugs, and so on. One way to make sure this doesn't happen is to specify that the field numbers (and/or names, which can also cause issues for JSON serialization) of your deleted fields are reserved. The protocol buffer compiler will complain if any future users try to use these field identifiers.

如果你通过完整地移除或注释一个字段，来[更新](https://developers.google.com/protocol-buffers/docs/proto3#updating)一个消息类型，未来的用户在对这个类型进行他们自己的更新时，就可以再次使用这个字段的编号。假如他们随后加载了同样的、旧版本的 `.proto` 文件，就可能会导致严重的问题，包括：数据损坏，隐私漏洞等等。能够确保这类严重问题不会发生的一个办法是：把你删除的字段的字段编号（和/或名称——可能也会在 JSON 序列化时导致一些问题）指定为 `reserved`（按即保留的）。任何未来的用户如果尝试使用这些字段的标识符，protocol buffer 编译器就会报错。

```proto
message Foo {
  reserved 2, 15, 9 to 11;
  reserved "foo", "bar";
}

```

Note that you can't mix field names and field numbers in the same reserved statement.

请注意，在同一个保留语句中，你不能混搭字段名称和字段编号。

## What's Generated From Your .proto?

## 你的 .proto 生成了什么？

When you run the protocol buffer compiler on a .proto, the compiler generates the code in your chosen language you'll need to work with the message types you've described in the file, including getting and setting field values, serializing your messages to an output stream, and parsing your messages from an input stream.

当你在一个 `.proto` 文件上运行 [protocol buffer 编译器](https://developers.google.com/protocol-buffers/docs/proto3#generating)时，编译器会根据你在文件中描述的消息类型，生成你所选择的、工作要用到的语言的代码，包括获取和设置字段值，序列化你的消息到一个输出流，以及从一个输入流解析你的消息。

For C++, the compiler generates a .h and .cc file from each .proto, with a class for each message type described in your file.

对于 **C++**，编译器由每个 `.proto` 文件生成一个 `.h` 和 `.cc` 文件，你的文件中描述的每个消息类型，都有一个与之对应的类。

For Java, the compiler generates a .java file with a class for each message type, as well as a special Builder classes for creating message class instances.

对于 **Java**，编译器生成一个 `.java` 文件，每个消息类型对应一个类，还有一个特殊的构建类用来创建消息类的实例对象。

Python is a little different – the Python compiler generates a module with a static descriptor of each message type in your .proto, which is then used with a metaclass to create the necessary Python data access class at runtime.

**Python** 则有一点不同——Python 编译器由你的 `.proto` 文件生成一个模块，`.proto` 文件中的每个消息类型在模块里都有一个与之对应的静态描述器。然后这个模块和一个元类一起使用，以在运行时创建必要的 Python 数据访问类。

For Go, the compiler generates a .pb.go file with a type for each message type in your file.

对于 **Go**，编译器生成一个 `.pb.go` 文件，你的文件中的每个消息类型在其中都有一个与之对应的类型。

For Ruby, the compiler generates a .rb file with a Ruby module containing your message types.

对于 **Ruby**，编译器生成一个 `.rb` 文件，其中的 Ruby 模块包含了你所有的消息类型。

For Objective-C, the compiler generates a pbobjc.h and pbobjc.m file from each .proto, with a class for each message type described in your file.

对于 **Objective-C**，编译器由每个 `.proto` 文件生成一个 `pbobjc.h` 和 `pbobjc.m` 文件，你的文件中的每个消息类型，都有一个与之对应的类。

For C#, the compiler generates a .cs file from each .proto, with a class for each message type described in your file.

对于 **C#**，编译器由每个 `.proto` 文件生成一个 `.cs` 文件，你的文件中描述的每个消息类型，都有一个与之对应的类。

For Dart, the compiler generates a .pb.dart file with a class for each message type in your file.

对于 **Dart**，编译器生成一个 `.pb.dart` 文件，你的文件中的每个消息类型，在其中都有一个与之对应的类。

You can find out more about using the APIs for each language by following the tutorial for your chosen language (proto3 versions coming soon). For even more API details, see the relevant API reference (proto3 versions also coming soon).

关于每种语言如何使用 API，你可以在相应语言版本的教程中查找到（proto3 的版本将很快到来）更多信息。如果是更多 API 的细节，则请查看相关的 [API 参考](https://developers.google.com/protocol-buffers/docs/reference/overview)（proto3 的版本也将很快到来）。
