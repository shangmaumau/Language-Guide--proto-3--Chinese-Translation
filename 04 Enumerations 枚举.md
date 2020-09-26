
# Enumerations

# 枚举

When you're defining a message type, you might want one of its fields to only have one of a pre-defined list of values. For example, let's say you want to add a corpus field for each SearchRequest, where the corpus can be UNIVERSAL, WEB, IMAGES, LOCAL, NEWS, PRODUCTS or VIDEO. You can do this very simply by adding an enum to your message definition with a constant for each possible value.

在你定义一个消息类型时，你可能想让它的某个字段，只能是一组预先定义好的值中的某一个。举个例子，比方说你想为每个 `SearchRequest` 添加一个 `corpus` 字段，这个 `corpus` 可以是 `UNIVERSAL`，`WEB`，`IMAGES`，`LOCAL`，`NEWS`，`PRODUCTS` 或 `VIDEO`。你可以通过向你的消息定义中添加一个`枚举`来很轻松地做到这点，`枚举`中的每个可能的值，都有一个常量与之对应。

In the following example we've added an enum called Corpus with all the possible values, and a field of type Corpus:

在下面的示例中，我们已经添加了一个叫做 `Corpus` 的`枚举`——附带了它所有可能的值——，以及一个类型为 `Corpus` 的字段：

```
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  Corpus corpus = 4;
}

```

As you can see, the Corpus enum's first constant maps to zero: every enum definition must contain a constant that maps to zero as its first element. This is because:

正如你看到的那样，`Corpus` 枚举的第一个常量映射到了〇：每个枚举定义**必须**包含一个映射到〇的常量以作为它的第一个元素。这是因为：

* There must be a zero value, so that we can use 0 as a numeric default value.

* 必须有一个〇值，因而我们才可以使用 0 作为数字类型的[默认值](https://developers.google.com/protocol-buffers/docs/proto3#default)。

* The zero value needs to be the first element, for compatibility with the proto2 semantics where the first enum value is always the default.

* 〇值需要是第一个元素，以保证与 [proto2](https://developers.google.com/protocol-buffers/docs/proto) 语法的兼容性——其第一个枚举的值总是默认值。

You can define aliases by assigning the same value to different enum constants. To do this you need to set the allow_alias option to true, otherwise the protocol compiler will generate an error message when aliases are found.

通过将相同的值分配给不同的枚举常量，你就可以定义别名了。要做到这点，你需要设置 `allow_alias` 选项为 `ture`，否则当 protocol 编译器发现别名时，就会生成一条错误信息。

```proto
message MyMessage1 {
  enum EnumAllowingAlias {
    option allow_alias = true;
    UNKNOWN = 0;
    STARTED = 1;
    RUNNING = 1;
  }
}
message MyMessage2 {
  enum EnumNotAllowingAlias {
    UNKNOWN = 0;
    STARTED = 1;
    // RUNNING = 1;  // Uncommenting this line will cause a compile error inside Google and a warning message outside.
    // 取消注释本行就会导致在 Google 【实现代码】内部的编译错误，以及外部的警告消息。
  }
}

```

Enumerator constants must be in the range of a 32-bit integer. Since enum values use varint encoding on the wire, negative values are inefficient and thus not recommended. You can define enums within a message definition, as in the above example, or outside – these enums can be reused in any message definition in your .proto file. You can also use an enum type declared in one message as the type of a field in a different message, using the syntax `_MessageType_._EnumType_`.

枚举常量必须在 32 位的整数范围之内。因为`枚举`值在通信线路上使用了[变长整型编码（varint encoding）](https://developers.google.com/protocol-buffers/docs/encoding)，而负值效率低，因此不推荐。你可以在一个消息定义的内部定义`枚举`，像上面的示例那样，或者外部——在你的 `.proto` 文件中，这些`枚举`可以在任一消息类型定义中复用。你也可以使用一个已经在某个消息中声明了的`枚举`类型，来作为不同的消息的某个字段的类型，用这个句法：`_MessageType_._EnumType_`。

When you run the protocol buffer compiler on a .proto that uses an enum, the generated code will have a corresponding enum for Java or C++, a special EnumDescriptor class for Python that's used to create a set of symbolic constants with integer values in the runtime-generated class.

当你在一个使用了`枚举`的 `.proto` 上运行 protocol buffer 编译器时，对于 Java 或 C++，生成的代码会有一个对应的枚举，对于 Python，则会有一个特殊的枚举描述器（EnumDescriptor）类，用于在运行时生成的类中，创建一系列附带着整数值的符号常量。

**Caution:** the generated code may be subject to language-specific limitations on the number of enumerators (low thousands for one language). Please review the limitations for the languages you plan to use.

**⚠️警告**：生成的代码可能取决于特定语言在枚举数量上的限制（一个语言少的话只有上千个）。请评估你打算使用的语言的限制数量。

During deserialization, unrecognized enum values will be preserved in the message, though how this is represented when the message is deserialized is language-dependent. In languages that support open enum types with values outside the range of specified symbols, such as C++ and Go, the unknown enum value is simply stored as its underlying integer representation. In languages with closed enum types such as Java, a case in the enum is used to represent an unrecognized value, and the underlying integer can be accessed with special accessors. In either case, if the message is serialized the unrecognized value will still be serialized with the message.

在反序列化期间，不能识别的枚举值会被保存在消息中，但是这些值在消息被反序列化时会如何呈现，却是因语言而异的了。在支持开放型枚举类型的值可以在指定符号范围之外的语言，像 C++ 和 Go 中，未知的枚举值只是按照其基本整数呈现（underlying integer representation）来存储。而在闭合型枚举类型的语言像 Java 中，不能识别的值会使用枚举中的某个条目（case）来表示，并且其基本整数（underlying integer）可通过特殊的访问器来获取到。在其他的情形下，如果消息被序列化了，不能识别的值仍会同消息一起被序列化。


For more information about how to work with message enums in your applications, see the generated code guide for your chosen language.

有关如何在你的应用程序中使用消息`枚举`的更多信息，请查看与你选择的语言对应的[生成代码指南](https://developers.google.com/protocol-buffers/docs/reference/overview)。


## Reserved Values

## 保留值

If you update an enum type by entirely removing an enum entry, or commenting it out, future users can reuse the numeric value when making their own updates to the type. This can cause severe issues if they later load old versions of the same .proto, including data corruption, privacy bugs, and so on. One way to make sure this doesn't happen is to specify that the numeric values (and/or names, which can also cause issues for JSON serialization) of your deleted entries are reserved. The protocol buffer compiler will complain if any future users try to use these identifiers. You can specify that your reserved numeric value range goes up to the maximum possible value using the max keyword.

如果你通过完整地移除或注释一个枚举条目（entry）来[更新](https://developers.google.com/protocol-buffers/docs/proto3#updating)一个枚举类型，未来的用户在对这个类型进行他们自己的更新时，就可以再次使用这些数字值。如果他们随后加载同样的、旧版本的 `.proto` 文件，就可能会导致严重的问题，包括数据损坏，隐私漏洞等等。能够确保这类严重问题不会发生的一个办法是，把你删除的条目的数字值指定为（和/或名称——也会在 JSON 序列化时引起问题）为 `reserved`。任何未来的用户如果尝试使用这些标识符，protocol buffer 编译器就会报错。你可以使用 `max` 关键字来指定你的保留的数字值可上达的最大可能值的范围。

```proto
enum Foo {
  reserved 2, 15, 9 to 11, 40 to max;
  reserved "FOO", "BAR";
}

```

Note that you can't mix field names and numeric values in the same reserved statement.

请注意，在同一条 `reserved` 声明中，你不能混搭字段名称和数字值。
