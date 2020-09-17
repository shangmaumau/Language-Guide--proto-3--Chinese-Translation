12 Packages.md

Packages

You can add an optional package specifier to a .proto file to prevent name clashes between protocol message types.

package foo.bar;
message Open { ... }

You can then use the package specifier when defining fields of your message type:

message Foo {
  ...
  foo.bar.Open open = 1;
  ...
}

The way a package specifier affects the generated code depends on your chosen language:

    In C++ the generated classes are wrapped inside a C++ namespace. For example, Open would be in the namespace foo::bar.
    In Java, the package is used as the Java package, unless you explicitly provide an option java_package in your .proto file.
    In Python, the package directive is ignored, since Python modules are organized according to their location in the file system.
    In Go, the package is used as the Go package name, unless you explicitly provide an option go_package in your .proto file.
    In Ruby, the generated classes are wrapped inside nested Ruby namespaces, converted to the required Ruby capitalization style (first letter capitalized; if the first character is not a letter, PB_ is prepended). For example, Open would be in the namespace Foo::Bar.
    In C# the package is used as the namespace after converting to PascalCase, unless you explicitly provide an option csharp_namespace in your .proto file. For example, Open would be in the namespace Foo.Bar.

Packages and Name Resolution

Type name resolution in the protocol buffer language works like C++: first the innermost scope is searched, then the next-innermost, and so on, with each package considered to be "inner" to its parent package. A leading '.' (for example, .foo.bar.Baz) means to start from the outermost scope instead.

The protocol buffer compiler resolves all type names by parsing the imported .proto files. The code generator for each language knows how to refer to each type in that language, even if it has different scoping rules.