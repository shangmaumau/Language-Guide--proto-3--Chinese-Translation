12 Packages.md

# Packages

# 包

You can add an optional package specifier to a .proto file to prevent name clashes between protocol message types.

你可以给一个 .proto 文件添加一个可选的包区分符来在 protocol 消息类型间预防命名冲突。

```proto
package foo.bar;
message Open { ... }
```

You can then use the package specifier when defining fields of your message type:

然后在定义你的消息类型的字段时，你就可以使用这个包区分符：

```proto
message Foo {
  ...
  foo.bar.Open open = 1;
  ...
}
```

The way a package specifier affects the generated code depends on your chosen language:

一个包区分符对生成的代码的影响的（具体）方式取决于你选择的语言：

* In C++ the generated classes are wrapped inside a C++ namespace. For example, Open would be in the namespace foo::bar.
* In Java, the package is used as the Java package, unless you explicitly provide an option java_package in your .proto file.
* In Python, the package directive is ignored, since Python modules are organized according to their location in the file system.
* In Go, the package is used as the Go package name, unless you explicitly provide an option go_package in your .proto file.
* In Ruby, the generated classes are wrapped inside nested Ruby namespaces, converted to the required Ruby capitalization style (first letter capitalized; if the first character is not a letter, PB_ is prepended). For example, Open would be in the namespace Foo::Bar.
* In C# the package is used as the namespace after converting to PascalCase, unless you explicitly provide an option csharp_namespace in your .proto file. For example, Open would be in the namespace Foo.Bar.

* 在 C++ 中，生成的类们包裹在一个 C++ 命名空间内部。例如，Open 会在 foo::bar 命名空间内。
* 在 Java 中，包会被作为 Java 包来使用，除非你在你的 .proto 文件中显式地提供一个 java_package 选项。
* 在 Python 里，包指令被忽略，因为 Python 模块会根据它们在文件系统中的位置来管理。
* 在 Go 里，包会被作为 Go 包名称来使用，除非你在你的 .proto 文件中显式地提供一个 go_package 选项。

Packages and Name Resolution

Type name resolution in the protocol buffer language works like C++: first the innermost scope is searched, then the next-innermost, and so on, with each package considered to be "inner" to its parent package. A leading '.' (for example, .foo.bar.Baz) means to start from the outermost scope instead.

The protocol buffer compiler resolves all type names by parsing the imported .proto files. The code generator for each language knows how to refer to each type in that language, even if it has different scoping rules.