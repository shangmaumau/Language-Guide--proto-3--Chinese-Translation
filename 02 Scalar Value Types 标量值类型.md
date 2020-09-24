
# Scalar Value Types

# 标量值类型

A scalar message field can have one of the following types – the table shows the type specified in the .proto file, and the corresponding type in the automatically generated class:

一个标量消息字段可以是下述类型中的一个——下表展示了在 .proto 文件中指定的类型，以及在自动生成的类中相对应的类型：

| .proto Type |	Notes |	C++ Type | Java Type | Python Type[2] | Go Type | Ruby Type | C# Type | PHP Type | Dart Type |
| :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- |
| double |  | double | double | float | float64 | Float | double | float | double |
| float |  | float | float | float | float32 | Float | float | float | double | 
| int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| uint32 | Uses variable-length encoding. | uint32 | int[1] | int/long[3] | uint32 | Fixnum or Bignum (as required) | uint | integer | int |
| uint64 | Uses variable-length encoding. | uint64 | long[1] | int/long[3] | uint64 | Bignum | ulong | integer/string[5] | Int64 |
| sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| fixed32 | Always four bytes. More efficient than uint32 if values are often greater than $2^{28}$. | uint32 | int[1] | int/long[3] | uint32 | Fixnum or Bignum (as required) | uint | integer | int |
| fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than $2^{56}$. | uint64 | long[1] | int/long[3] | uint64 | Bignum | ulong | integer/string[5] | Int64 |
| sfixed32 | Always four bytes. | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| sfixed64 | Always eight bytes. | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| bool |  | bool | boolean | bool | bool | TrueClass/FalseClass | bool | boolean | bool |
| string | A string must always contain UTF-8 encoded or 7-bit ASCII text, and cannot be longer than $2^{32}$. | string | String | str/unicode[4] | string | String (UTF-8) | string | string | String |
| bytes | May contain any arbitrary sequence of bytes no longer than $2^{32}$. | string | ByteString | str | []byte | String (ASCII-8BIT) | ByteString | string | List |

| .proto Type |	Notes |	C++ Type | Java Type | Python Type[2] | Go Type | Ruby Type | C# Type | PHP Type | Dart Type |
| :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- |
| double |  | double | double | float | float64 | Float | double | float | double |
| float |  | float | float | float | float32 | Float | float | float | double | 
| int32 | 使用变长编码。编码负数效率低——如果你的字段可能有负值，请换用 sint32。 | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| int64 | 使用变长编码。编码负数效率低——如果你的字段可能有负值，请换用 sint64。 | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| uint32 | 使用变长编码。 | uint32 | int[1] | int/long[3] | uint32 | Fixnum or Bignum (as required) | uint | integer | int |
| uint64 | 使用变长编码。 | uint64 | long[1] | int/long[3] | uint64 | Bignum | ulong | integer/string[5] | Int64 |
| sint32 | 使用变长编码。有符号的 int 值。编码负值时，比普通的 int32 类整数类型具有更高的效率。 | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| sint64 | 使用变长编码。有符号的 int 值。编码负值时，比普通的 int64 类整数类型具有更高的效率。 | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| fixed32 | 恒为四个字节。如果值总是大于 $2^{28}$，则比 uint32 具有更高的效率。 | uint32 | int[1] | int/long[3] | uint32 | Fixnum or Bignum (as required) | uint | integer | int |
| fixed64 | 恒为八个字节。如果值总是大于 $2^{56}$，则比 uint64 具有更高的效率。 | uint64 | long[1] | int/long[3] | uint64 | Bignum | ulong | integer/string[5] | Int64 |
| sfixed32 | 恒为四个字节。 | int32 | int | int | int32 | Fixnum or Bignum (as required) | int | integer | int |
| sfixed64 | 恒为八个字节。 | int64 | long | int/long[3] | int64 | Bignum | long | integer/string[5] | Int64 |
| bool |  | bool | boolean | bool | bool | TrueClass/FalseClass | bool | boolean | bool |
| string | 字符串必须总是包含 UTF-8 编码的或七位的 ASCII 的文本，并且长度不能超过 $2^{32}$。| string | String | str/unicode[4] | string | String (UTF-8) | string | string | String |
| bytes | 可能包含任何任意序列的字节码，其长度不能超过 $2^{32}$。 | string | ByteString | str | []byte | String (ASCII-8BIT) | ByteString | string | List |

You can find out more about how these types are encoded when you serialize your message in Protocol Buffer Encoding.

有关这些类型在你序列化你的消息时是如何编码的，你可以在 [Protocol Buffer 编码](https://developers.google.com/protocol-buffers/docs/encoding)中查找到更多信息。

[1] In Java, unsigned 32-bit and 64-bit integers are represented using their signed counterparts, with the top bit simply being stored in the sign bit.

[1] 在 Java 中，无符号 32 位和 64 位的整数使用它们的有符号的对应类型来表示，其中最高位只是简单地在符号位存储着。

[2] In all cases, setting values to a field will perform type checking to make sure it is valid.

[2] 在所有情形中，给一个字段设置值的时候，都会执行类型检查，以确保值是有效的。

[3] 64-bit or unsigned 32-bit integers are always represented as long when decoded, but can be an int if an int is given when setting the field. In all cases, the value must fit in the type represented when set. See [2].

[3] 在解码时，64 位或无符号 32 位整数总是由 long 来表示，但假如在设置这个字段的时候，给的是一个 int 类型的值，也可以是 int。在所有情形中，当设置字段值时，值的类型必须符合其要表示的类型（**按**就是设置的值的类型，必须与生成的类中相应的字段的类型保持一致）。见第二条。

[4] Python strings are represented as unicode on decode but can be str if an ASCII string is given (this is subject to change).

[4] 解码过程中，Python 的字符串用 unicode 来表示，但假如给的是一个 ASCII 的字符串，也可以用 str 来表示（本条可能会调整）。

[5] Integer is used on 64-bit machines and string is used on 32-bit machines.

[5] 64 位的机器上使用整数，32 位的机器上使用字符串。