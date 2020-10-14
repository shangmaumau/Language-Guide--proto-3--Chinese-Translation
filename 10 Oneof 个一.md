
# Oneof

# 个一

If you have a message with many fields and where at most one field will be set at the same time, you can enforce this behavior and save memory by using the oneof feature.

Oneof fields are like regular fields except all the fields in a oneof share memory, and at most one field can be set at the same time. Setting any member of the oneof automatically clears all the other members. You can check which value in a oneof is set (if any) using a special case() or WhichOneof() method, depending on your chosen language.


如果你有一个拥有许多字段的消息，且同一次最多有一个字段会被设置，你可以通过使用 oneof 功能来强制此种行为并节省内存。

Oneof 字段像普通的字段一样，唯一不同的是一个 oneof 中的所有字段共享（同一段）内存，且同一次最多只能设置一个字段。设置 oneof 中的任一个成员都会自动清空所有其他成员。你可以使用一个特殊的 `case()` 或 `WhichOneof()` 方法来检查 oneof 中的哪个值（如果有的话）被设置过了，（具体的方法）取决于你选择的语言。

## Using Oneof

## 使用 Oneof

To define a oneof in your .proto you use the oneof keyword followed by your oneof name, in this case test_oneof:

要在你的 `.proto` 中定义一个 oneof，你使用 `oneof` 关键字，你的 oneof 名称紧随其后，在这个例子 `test_oneof` 中：

```proto
message SampleMessage {
  oneof test_oneof {
    string name = 4;
    SubMessage sub_message = 9;
  }
}
```

You then add your oneof fields to the oneof definition. You can add fields of any type, except map fields and repeated fields.

In your generated code, oneof fields have the same getters and setters as regular fields. You also get a special method for checking which value (if any) in the oneof is set. You can find out more about the oneof API for your chosen language in the relevant API reference.


你随后添加你的 oneof 字段到 oneof 定义中。你可以添加任意类型的字段——除了 `map` 字段和 `repeated` 字段。

在你的生成代码中，oneof 字段与普通的字段一样，有相同的访问器和设置器。你也获得一个特殊的方法来检查 oneof 中的哪个值（如果有的话）被设置过了。有关你选择的语言的 oneof API 的更多信息，你可以在相关 [API 参考](https://developers.google.com/protocol-buffers/docs/reference/overview)中查看到。


## Oneof Features

## Oneof 功能


* Setting a oneof field will automatically clear all other members of the oneof. So if you set several oneof fields, only the last field you set will still have a value.

* 设置一个 oneof 字段会自动清空所有其他 oneof 的成员。因此，如果你设置若干 oneof 字段，仅有你设置的*最后*一个字段仍会有一个值（其他的都被清空了）。
    ```cpp
    SampleMessage message;
    message.set_name("name");
    CHECK(message.has_name());
    message.mutable_sub_message();   // Will clear name field. 会清空 name 字段。
    CHECK(!message.has_name());
    ```

 * If the parser encounters multiple members of the same oneof on the wire, only the last member seen is used in the parsed message.

* A oneof cannot be repeated.

* Reflection APIs work for oneof fields.

* If you set a oneof field to the default value (such as setting an int32 oneof field to 0), the "case" of that oneof field will be set, and the value will be serialized on the wire.

* If you're using C++, make sure your code doesn't cause memory crashes. The following sample code will crash because sub_message was already deleted by calling the set_name() method.

* 如果解析器在通信线路上遇到了同一个 oneof 的多个成员，只有最后一个看到的成员会被用在解析过的消息中。

* Oneof 不能是 `repeated`。

* 映射 APIs 可与 oneof 字段工作。

* 如果你把一个 oneof 字段设为其默认值（比如设置一个 `int32` oneof 字段为 0），那个 oneof 字段的“条目”（"case"）就会被设置，且这个值在通信线路上会被序列化。

* 如果你正在使用 C++，请确保你的代码不会导致内存崩溃。下面的示例代码会崩溃，因为 `sub_message` 已经（被你）通过调用 `set_name()` 方法删除了。

    ```cpp
    SampleMessage message;
    SubMessage* sub_message = message.mutable_sub_message();
    message.set_name("name");      // Will delete sub_message 会删除 sub_message
    sub_message->set_...            // Crashes here 在这里崩溃
    ```

* Again in C++, if you Swap() two messages with oneofs, each message will end up with the other’s oneof case: in the example below, msg1 will have a sub_message and msg2 will have a name.

* 同样在 C++ 中，如果你用（多个） oneof(s) `Swap()` 了两条消息， 每条消息都会以另一个的 oneof 条目（case）结尾：在下面示例中，`msg1` 会有一个 `sub_message` 且 `msg2` 会有一个 `name` 。

    ```cpp
    SampleMessage msg1;
    msg1.set_name("name");
    SampleMessage msg2;
    msg2.mutable_sub_message();
    msg1.swap(&msg2);
    CHECK(msg1.has_sub_message());
    CHECK(msg2.has_name());
    ```

## Backwards-compatibility issues

## 向后兼容性问题

Be careful when adding or removing oneof fields. If checking the value of a oneof returns None/NOT_SET, it could mean that the oneof has not been set or it has been set to a field in a different version of the oneof. There is no way to tell the difference, since there's no way to know if an unknown field on the wire is a member of the oneof.

在添加或移除 oneof 字段时请务必小心。如果检查一个 oneof 的值返回了 `None` 或 `NOT_SET`，它可能意味着 oneof 尚未被设置过或者它已在一个不同版本的 oneof 中被设置过。毫无办法来分辨其不同，因为毫无办法知道一个通信线路上的未知字段是否是 oneof 的一个成员。

### Tag Reuse Issues

### 标记重用问题

Move fields into or out of a oneof: You may lose some of your information (some fields will be cleared) after the message is serialized and parsed. However, you can safely move a single field into a new oneof and may be able to move multiple fields if it is known that only one is ever set.

Delete a oneof field and add it back: This may clear your currently set oneof field after the message is serialized and parsed.

Split or merge oneof: This has similar issues to moving regular fields.

* **移入或移出 oneof 的字段**：消息序列化并解析后，你可能会丢失你的一部分信息（一些字段会被清空）。不过，你可以安全地移动一个单独的字段到一个**新的** oneof 中，且如果已知不管任何时候仅有一个字段会被设置，还可以移除多条字段。

* **删除一个 oneof 字段然后再添加回来**：消息序列化并解析后，这会清空掉你的 oneof 字段的已有设置。

* **分离或合并 oneof**：这和移动普通字段（一样）有类似的问题。