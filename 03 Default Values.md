03 Default Values

Default Values

When a message is parsed, if the encoded message does not contain a particular singular element, the corresponding field in the parsed object is set to the default value for that field. These defaults are type-specific:

    For strings, the default value is the empty string.
    For bytes, the default value is empty bytes.
    For bools, the default value is false.
    For numeric types, the default value is zero.
    For enums, the default value is the first defined enum value, which must be 0.
    For message fields, the field is not set. Its exact value is language-dependent. See the generated code guide for details.

The default value for repeated fields is empty (generally an empty list in the appropriate language).

Note that for scalar message fields, once a message is parsed there's no way of telling whether a field was explicitly set to the default value (for example whether a boolean was set to false) or just not set at all: you should bear this in mind when defining your message types. For example, don't have a boolean that switches on some behaviour when set to false if you don't want that behaviour to also happen by default. Also note that if a scalar message field is set to its default, the value will not be serialized on the wire.

See the generated code guide for your chosen language for more details about how defaults work in generated code.