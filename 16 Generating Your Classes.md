16 Generating Your Classes

Generating Your Classes

To generate the Java, Python, C++, Go, Ruby, Objective-C, or C# code you need to work with the message types defined in a .proto file, you need to run the protocol buffer compiler protoc on the .proto. If you haven't installed the compiler, download the package and follow the instructions in the README. For Go, you also need to install a special code generator plugin for the compiler: you can find this and installation instructions in the golang/protobuf repository on GitHub.

The Protocol Compiler is invoked as follows:

protoc --proto_path=IMPORT_PATH --cpp_out=DST_DIR --java_out=DST_DIR --python_out=DST_DIR --go_out=DST_DIR --ruby_out=DST_DIR --objc_out=DST_DIR --csharp_out=DST_DIR path/to/file.proto

    IMPORT_PATH specifies a directory in which to look for .proto files when resolving import directives. If omitted, the current directory is used. Multiple import directories can be specified by passing the --proto_path option multiple times; they will be searched in order. -I=_IMPORT_PATH_ can be used as a short form of --proto_path.
    You can provide one or more output directives:
        --cpp_out generates C++ code in DST_DIR. See the C++ generated code reference for more.
        --java_out generates Java code in DST_DIR. See the Java generated code reference for more.
        --python_out generates Python code in DST_DIR. See the Python generated code reference for more.
        --go_out generates Go code in DST_DIR. See the Go generated code reference for more.
        --ruby_out generates Ruby code in DST_DIR. Ruby generated code reference is coming soon!
        --objc_out generates Objective-C code in DST_DIR. See the Objective-C generated code reference for more.
        --csharp_out generates C# code in DST_DIR. See the C# generated code reference for more.
        --php_out generates PHP code in DST_DIR. See the PHP generated code reference for more.As an extra convenience, if the DST_DIR ends in .zip or .jar, the compiler will write the output to a single ZIP-format archive file with the given name. .jar outputs will also be given a manifest file as required by the Java JAR specification. Note that if the output archive already exists, it will be overwritten; the compiler is not smart enough to add files to an existing archive.
    You must provide one or more .proto files as input. Multiple .proto files can be specified at once. Although the files are named relative to the current directory, each file must reside in one of the IMPORT_PATHs so that the compiler can determine its canonical name.
