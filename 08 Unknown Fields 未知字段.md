
# Unknown Fields

# 未知字段

Unknown fields are well-formed protocol buffer serialized data representing fields that the parser does not recognize. For example, when an old binary parses data sent by a new binary with new fields, those new fields become unknown fields in the old binary.

未知字段是指解析器不能识别的、形式合法的 protocol buffer 序列化后的数据呈现字段。例如，当一个旧二进制去解析由带着新字段的新二进制发送的数据时，这些新字段在旧二进制中就成了未知字段。

Originally, proto3 messages always discarded unknown fields during parsing, but in version 3.5 we reintroduced the preservation of unknown fields to match the proto2 behavior. In versions 3.5 and later, unknown fields are retained during parsing and included in the serialized output.

最初，proto3 消息总是在解析期间丢弃掉未知字段，但在 3.5 版中我们重新引入了对未知字段的留存机制（preservation），来匹配 proto2 的行为。在 3.5 及之后的版本中，未知字段在解析期间会被保留和包含在序列化后的输入中。