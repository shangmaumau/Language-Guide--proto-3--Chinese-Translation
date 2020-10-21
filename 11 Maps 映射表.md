
# Maps

# 映射表

If you want to create an associative map as part of your data definition, protocol buffers provides a handy shortcut syntax:

如果你想创建一个关联映射表作为你的数据定义的一部分，protocol buffers 提供了一个便利的快捷句法：

```proto
map<key_type, value_type> map_field = N;
```

...where the key_type can be any integral or string type (so, any scalar type except for floating point types and bytes). Note that enum is not a valid key_type. The value_type can be any type except another map.

……其中 `key_type` 可以是任何整数或字符串类型（即，除浮点类型和`字节码（bytes）`之外的任何[标量](https://developers.google.com/protocol-buffers/docs/proto3#scalar)类型）。注意，枚举并非一个有效的 `key_type` 。`value_type` 可以是除别一种映射表之外的任何类型。

So, for example, if you wanted to create a map of projects where each Project message is associated with a string key, you could define it like this:

那么，举例来说，如果你想创建项目们——每个 `Project` 消息都与一个字符串键名相关联——的一个映射表，你可以像这样定义它：

```proto
map<string, Project> projects = 3;
```

* Map fields cannot be repeated.
* Wire format ordering and map iteration ordering of map values is undefined, so you cannot rely on your map items being in a particular order.
* When generating text format for a .proto, maps are sorted by key. Numeric keys are sorted numerically.
* When parsing from the wire or when merging, if there are duplicate map keys the last key seen is used. When parsing a map from text format, parsing may fail if there are duplicate keys.
* If you provide a key but no value for a map field, the behavior when the field is serialized is language-dependent. In C++, Java, and Python the default value for the type is serialized, while in other languages nothing is serialized.

* 映射表的字段不能是 `repeated` 。
* 映射表值的通信线路形式排序和映射表迭代排序是不明确的，因此你不能信赖（rely on）你的映射表条目们处于一个特定的序列中。
* 当为一个 `.proto` 生成文本形式时，映射表们通过键名来排序。数字键数字地来排序（**按**就是按照数字大小来排序，这里应该是由小到大）。
* 当正从通信线路上解析或合并时，如果有重复的映射表键名，则使用最后一个看到的。当正从文本形式解析一个映射表时，如果有重复的键名解析就会失败。
* 如果你为一个映射表字段提供了一个键名但没有（对应的）值，在字段被序列化时的行为就是因语言而异的了。在 C++、Java 和 Python 中，那个类型的默认值会被序列化，但在其他语言中，啥也没有被序列化。

The generated map API is currently available for all proto3 supported languages. You can find out more about the map API for your chosen language in the relevant API reference.

生成的映射表 API 当前对所有 proto3 支持的语言都是可用的。有关你选择的语言的映射表 API 的更多信息，你可以在相关 [API 参考](https://developers.google.com/protocol-buffers/docs/reference/overview)中查看到。

## Backwards compatibility

## 向后兼容性

The map syntax is equivalent to the following on the wire, so protocol buffers implementations that do not support maps can still handle your data:

映射表句法在通信线路上，和下面（的形式）等同，因此不支持映射表的 protocol buffers 实现仍能处理你的数据：

```proto
message MapFieldEntry {
  key_type key = 1;
  value_type value = 2;
}

repeated MapFieldEntry map_field = N;
```

Any protocol buffers implementation that supports maps must both produce and accept data that can be accepted by the above definition.

任何支持映射表的 protocol buffers 实现必须既能生产也能接收可被上面定义接收的数据。