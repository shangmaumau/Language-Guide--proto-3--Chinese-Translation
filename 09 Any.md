09 Any

Any

The Any message type lets you use messages as embedded types without having their .proto definition. An Any contains an arbitrary serialized message as bytes, along with a URL that acts as a globally unique identifier for and resolves to that message's type. To use the Any type, you need to import google/protobuf/any.proto.

import "google/protobuf/any.proto";

message ErrorStatus {
  string message = 1;
  repeated google.protobuf.Any details = 2;
}

The default type URL for a given message type is type.googleapis.com/_packagename_._messagename_.

Different language implementations will support runtime library helpers to pack and unpack Any values in a typesafe manner – for example, in Java, the Any type will have special pack() and unpack() accessors, while in C++ there are PackFrom() and UnpackTo() methods:

// Storing an arbitrary message type in Any.
NetworkErrorDetails details = ...;
ErrorStatus status;
status.add_details()->PackFrom(details);

// Reading an arbitrary message from Any.
ErrorStatus status = ...;
for (const Any& detail : status.details()) {
  if (detail.Is<NetworkErrorDetails>()) {
    NetworkErrorDetails network_error;
    detail.UnpackTo(&network_error);
    ... processing network_error ...
  }
}

Currently the runtime libraries for working with Any types are under development.

If you are already familiar with proto2 syntax, the Any can hold arbitrary proto3 messages, similar to proto2 messages which can allow extensions.