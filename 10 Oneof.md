10 Oneof

Oneof

If you have a message with many fields and where at most one field will be set at the same time, you can enforce this behavior and save memory by using the oneof feature.

Oneof fields are like regular fields except all the fields in a oneof share memory, and at most one field can be set at the same time. Setting any member of the oneof automatically clears all the other members. You can check which value in a oneof is set (if any) using a special case() or WhichOneof() method, depending on your chosen language.
Using Oneof

To define a oneof in your .proto you use the oneof keyword followed by your oneof name, in this case test_oneof:

message SampleMessage {
  oneof test_oneof {
    string name = 4;
    SubMessage sub_message = 9;
  }
}

You then add your oneof fields to the oneof definition. You can add fields of any type, except map fields and repeated fields.

In your generated code, oneof fields have the same getters and setters as regular fields. You also get a special method for checking which value (if any) in the oneof is set. You can find out more about the oneof API for your chosen language in the relevant API reference.
Oneof Features

    Setting a oneof field will automatically clear all other members of the oneof. So if you set several oneof fields, only the last field you set will still have a value.

    SampleMessage message;
    message.set_name("name");
    CHECK(message.has_name());
    message.mutable_sub_message();   // Will clear name field.
    CHECK(!message.has_name());

If the parser encounters multiple members of the same oneof on the wire, only the last member seen is used in the parsed message.

A oneof cannot be repeated.

Reflection APIs work for oneof fields.

If you set a oneof field to the default value (such as setting an int32 oneof field to 0), the "case" of that oneof field will be set, and the value will be serialized on the wire.

If you're using C++, make sure your code doesn't cause memory crashes. The following sample code will crash because sub_message was already deleted by calling the set_name() method.

SampleMessage message;
SubMessage* sub_message = message.mutable_sub_message();
message.set_name("name");      // Will delete sub_message
sub_message->set_...            // Crashes here

Again in C++, if you Swap() two messages with oneofs, each message will end up with the other’s oneof case: in the example below, msg1 will have a sub_message and msg2 will have a name.

SampleMessage msg1;
msg1.set_name("name");
SampleMessage msg2;
msg2.mutable_sub_message();
msg1.swap(&msg2);
CHECK(msg1.has_sub_message());
CHECK(msg2.has_name());

Backwards-compatibility issues

Be careful when adding or removing oneof fields. If checking the value of a oneof returns None/NOT_SET, it could mean that the oneof has not been set or it has been set to a field in a different version of the oneof. There is no way to tell the difference, since there's no way to know if an unknown field on the wire is a member of the oneof.
Tag Reuse Issues

    Move fields into or out of a oneof: You may lose some of your information (some fields will be cleared) after the message is serialized and parsed. However, you can safely move a single field into a new oneof and may be able to move multiple fields if it is known that only one is ever set.
    Delete a oneof field and add it back: This may clear your currently set oneof field after the message is serialized and parsed.
    Split or merge oneof: This has similar issues to moving regular fields.