02 Scalar Value Types

Scalar Value Types

A scalar message field can have one of the following types – the table shows the type specified in the .proto file, and the corresponding type in the automatically generated class:
.proto Type 	Notes 	C++ Type 	Java Type 	Python Type[2] 	Go Type 	Ruby Type 	C# Type 	PHP Type 	Dart Type
double 		double 	double 	float 	float64 	Float 	double 	float 	double
float 		float 	float 	float 	float32 	Float 	float 	float 	double
int32 	Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. 	int32 	int 	int 	int32 	Fixnum or Bignum (as required) 	int 	integer 	int
int64 	Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. 	int64 	long 	int/long[3] 	int64 	Bignum 	long 	integer/string[5] 	Int64
uint32 	Uses variable-length encoding. 	uint32 	int[1] 	int/long[3] 	uint32 	Fixnum or Bignum (as required) 	uint 	integer 	int
uint64 	Uses variable-length encoding. 	uint64 	long[1] 	int/long[3] 	uint64 	Bignum 	ulong 	integer/string[5] 	Int64
sint32 	Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. 	int32 	int 	int 	int32 	Fixnum or Bignum (as required) 	int 	integer 	int
sint64 	Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. 	int64 	long 	int/long[3] 	int64 	Bignum 	long 	integer/string[5] 	Int64
fixed32 	Always four bytes. More efficient than uint32 if values are often greater than 228. 	uint32 	int[1] 	int/long[3] 	uint32 	Fixnum or Bignum (as required) 	uint 	integer 	int
fixed64 	Always eight bytes. More efficient than uint64 if values are often greater than 256. 	uint64 	long[1] 	int/long[3] 	uint64 	Bignum 	ulong 	integer/string[5] 	Int64
sfixed32 	Always four bytes. 	int32 	int 	int 	int32 	Fixnum or Bignum (as required) 	int 	integer 	int
sfixed64 	Always eight bytes. 	int64 	long 	int/long[3] 	int64 	Bignum 	long 	integer/string[5] 	Int64
bool 		bool 	boolean 	bool 	bool 	TrueClass/FalseClass 	bool 	boolean 	bool
string 	A string must always contain UTF-8 encoded or 7-bit ASCII text, and cannot be longer than 232. 	string 	String 	str/unicode[4] 	string 	String (UTF-8) 	string 	string 	String
bytes 	May contain any arbitrary sequence of bytes no longer than 232. 	string 	ByteString 	str 	[]byte 	String (ASCII-8BIT) 	ByteString 	string 	List

You can find out more about how these types are encoded when you serialize your message in Protocol Buffer Encoding.

[1] In Java, unsigned 32-bit and 64-bit integers are represented using their signed counterparts, with the top bit simply being stored in the sign bit.

[2] In all cases, setting values to a field will perform type checking to make sure it is valid.

[3] 64-bit or unsigned 32-bit integers are always represented as long when decoded, but can be an int if an int is given when setting the field. In all cases, the value must fit in the type represented when set. See [2].

[4] Python strings are represented as unicode on decode but can be str if an ASCII string is given (this is subject to change).

[5] Integer is used on 64-bit machines and string is used on 32-bit machines.