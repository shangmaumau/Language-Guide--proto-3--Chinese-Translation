
# Maps

# 映射表

If you want to create an associative map as part of your data definition, protocol buffers provides a handy shortcut syntax:

如果你想创建一个关联映射表作为你的数据定义的一部分，protocol buffers 提供了一个便利的快捷句法：

```proto
map<key_type, value_type> map_field = N;
```

...where the key_type can be any integral or string type (so, any scalar type except for floating point types and bytes). Note that enum is not a valid key_type. The value_type can be any type except another map.

……其中 key_type 可以是任意整数或字符串类型（即，除浮点类型和字节码之外的任意标量类型）。注意，枚举并非一个有效的 key_type。value_type 可以是除另外一种 map 之外的任意类型。

So, for example, if you wanted to create a map of projects where each Project message is associated with a string key, you could define it like this:


map<string, Project> projects = 3;

    Map fields cannot be repeated.
    Wire format ordering and map iteration ordering of map values is undefined, so you cannot rely on your map items being in a particular order.
    When generating text format for a .proto, maps are sorted by key. Numeric keys are sorted numerically.
    When parsing from the wire or when merging, if there are duplicate map keys the last key seen is used. When parsing a map from text format, parsing may fail if there are duplicate keys.
    If you provide a key but no value for a map field, the behavior when the field is serialized is language-dependent. In C++, Java, and Python the default value for the type is serialized, while in other languages nothing is serialized.

The generated map API is currently available for all proto3 supported languages. You can find out more about the map API for your chosen language in the relevant API reference.
Backwards compatibility

The map syntax is equivalent to the following on the wire, so protocol buffers implementations that do not support maps can still handle your data:

message MapFieldEntry {
  key_type key = 1;
  value_type value = 2;
}

repeated MapFieldEntry map_field = N;

Any protocol buffers implementation that supports maps must both produce and accept data that can be accepted by the above definition.