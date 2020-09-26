
# Nested Types

# 嵌套类型

You can define and use message types inside other message types, as in the following example – here the Result message is defined inside the SearchResponse message:

你可以定义并使用在其他消息类型内部的消息类型，像接下来的这个示例一样——这里 `Result` 消息定义在了 `SearchResponse` 消息的内部：

```proto
message SearchResponse {
  message Result {
    string url = 1;
    string title = 2;
    repeated string snippets = 3;
  }
  repeated Result results = 1;
}
```

If you want to reuse this message type outside its parent message type, you refer to it as \_Parent_.\_Type_:

如果你想在其父消息类型之外重复使用这个消息类型，你像这样引用它 `_Parent_._Type_`。

```proto
message SomeOtherMessage {
  SearchResponse.Result result = 1;
}
```

You can nest messages as deeply as you like:

你可以要多深就有多深地嵌套消息，只要你喜欢：

```proto
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

```