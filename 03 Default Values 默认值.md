03 Default Values

Default Values
默认值

When a message is parsed, if the encoded message does not contain a particular singular element, the corresponding field in the parsed object is set to the default value for that field. These defaults are type-specific:

当一个消息被解析时，如果编码过的消息并不包含一个特别的 singular 元素，在解析后的对象中，对应的字段就会设为那个字段的默认值。这些默认值是依类型而异的：


* For strings, the default value is the empty string.

* 对字符串类的，其默认值是空字符串。

* For bytes, the default value is empty bytes.

* 对字节类的，其默认值是空字节码。

* For bools, the default value is false.

* 对布尔类的，其默认值是 false。

* For numeric types, the default value is zero.

* 对数值类型的，其默认值是 0。

* For enums, the default value is the first defined enum value, which must be 0.

* 对[枚举类](https://developers.google.com/protocol-buffers/docs/proto3#enum)的，其默认值是**第一个定义的枚举的值**——必须为 0。

* For message fields, the field is not set. Its exact value is language-dependent. See the generated code guide for details.

* 对消息类的字段，其字段不会设置。字段的确切值，因语言而异。请查看[生成代码指南](https://developers.google.com/protocol-buffers/docs/reference/overview)来获取更多细节。

The default value for repeated fields is empty (generally an empty list in the appropriate language).

repeated 字段的默认值是空（通常是相关语言的一个空列表）。

Note that for scalar message fields, once a message is parsed there's no way of telling whether a field was explicitly set to the default value (for example whether a boolean was set to false) or just not set at all: you should bear this in mind when defining your message types. For example, don't have a boolean that switches on some behaviour when set to false if you don't want that behaviour to also happen by default. Also note that if a scalar message field is set to its default, the value will not be serialized on the wire.

请注意，对于标量消息字段，一旦一个消息解析过了，就无从分辨一个字段是显式地被设为了默认值（例如是否一个布尔被设为了 `false`），还是根本就没有设置：在定义你的消息类型时，你应该在脑海里记着这一点。举例来说，如果你不想让某种行为在默认情况下也会发生，就不要有一个当设为 `false` 时就开启这种行为的布尔（按因为布尔类型的字段，其默认值就是 `false`，很显然，这个默认值并不是你主动设置的，这意味着：这种行为是不可预知的）。还请注意，如果一个标量消息字段**被**设为了其类型的默认值，这个值在通信线路上就不会被序列化。

See the generated code guide for your chosen language for more details about how defaults work in generated code.

有关默认值在生成的代码中是如何工作的，请查看与你选择的语言对应的[生成代码指南](https://developers.google.com/protocol-buffers/docs/reference/overview)来获取更多细节。