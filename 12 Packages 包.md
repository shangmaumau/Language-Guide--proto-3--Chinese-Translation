12 Packages.md

# Packages

# 包

You can add an optional package specifier to a .proto file to prevent name clashes between protocol message types.

你可以给一个 `.proto` 文件添加一个可选的 `package` 区分符来在 protocol 消息类型间预防命名冲突。

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

* 在 **C++** 中，生成的类们包裹在一个 C++ 命名空间内部。例如，`Open` 会在 `foo::bar` 命名空间内。
* 在 **Java** 中，包会被作为 Java 包来使用，除非你在你的 `.proto` 文件中显式地提供一个 `option java_package` 。
* 在 **Python** 里，包指令被忽略，因为 Python 模块是按照它们在文件系统中的位置来管理的。
* 在 **Go** 里，包会被作为 Go 包名称来使用，除非你在你的 `.proto` 文件中显式地提供一个 `option go_package` 。
* 在 **Ruby** 中，生成的类们包裹在嵌套的 Ruby 命名空间内部，转换成规定的 Ruby 大写风格（首字母大写；如果第一个字符不是一个字母，则前缀 `PB_`）。例如，`Open` 会在 `Foo::Bar` 命名空间内。
* 在 **C#** 中，包在转换为帕斯卡命名（**按**每个单词的首字母大写，不区分是否在最前面）后被作为命名空间来使用，除非你在你的 `.proto` 文件中显式地提供一个 `option csharp_namespace` 。例如，`Open` 会在 `Foo.Bar` 命名空间内。

## Packages and Name Resolution

## 包与名称解析

Type name resolution in the protocol buffer language works like C++: first the innermost scope is searched, then the next-innermost, and so on, with each package considered to be "inner" to its parent package. A leading '.' (for example, .foo.bar.Baz) means to start from the outermost scope instead.

The protocol buffer compiler resolves all type names by parsing the imported .proto files. The code generator for each language knows how to refer to each type in that language, even if it has different scoping rules.

类型名称解析在 protocol buffer 语言中的工作方式类似 C++：首先最内层的作用域被搜索，然后是次一级的最内层，依此类推，每个包被认为在其父一级的包的“内部”。相反的，最前面有一个‘.’（例如，`.foo.bar.Baz`）则意味着从最外层的作用域开始。

protocol buffer 编译器通过解析导入的 `.proto` 文件来确定（resovle）所有的类型名称。每种语言的代码生成器知道怎样以那种语言来引用每种类型，即使它拥有不同的作用域规则。
