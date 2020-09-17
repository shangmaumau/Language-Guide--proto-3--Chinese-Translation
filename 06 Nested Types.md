06 Nested Types

Nested Types

You can define and use message types inside other message types, as in the following example – here the Result message is defined inside the SearchResponse message:

message SearchResponse {
  message Result {
    string url = 1;
    string title = 2;
    repeated string snippets = 3;
  }
  repeated Result results = 1;
}

If you want to reuse this message type outside its parent message type, you refer to it as _Parent_._Type_:

message SomeOtherMessage {
  SearchResponse.Result result = 1;
}

You can nest messages as deeply as you like:

message Outer {                  // Level 0
  message MiddleAA {  // Level 1
    message Inner {   // Level 2
      int64 ival = 1;
      bool  booly = 2;
    }
  }
  message MiddleBB {  // Level 1
    message Inner {   // Level 2
      int32 ival = 1;
      bool  booly = 2;
    }
  }
}