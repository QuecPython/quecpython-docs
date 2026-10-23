# Data Format Conversion

In Python, handling binary data is a common operation. MicroPython provides two modules, ustruct and ubinascii, for packing, unpacking, encoding, and decoding binary data. This article will introduce the functions of the ustruct and ubinascii modules and provide some usage examples.

## ustruct

The ustruct module is used for handling binary data in MicroPython. It can convert Python data types to binary data and vice versa. The ustruct module provides five functions: *pack*, *unpack*, *calcsize*, *pack_into*, and *unpack_from*.



### 1.1 Format Table Supported by ustruct

The formats supported by ustruct are listed in the table below:

| Format | C Type               | Python             | Bytes |
| ------ | -------------------- | ------------------ | ------ |
| `x`    | pad byte             | no value           | 1      |
| `c`    | `char`               | string of length 1 | 1      |
| `b`    | `signed char`        | integer            | 1      |
| `B`    | `unsigned char`      | integer            | 1      |
| `?`    | `_Bool`              | bool               | 1      |
| `h`    | `short`              | integer            | 2      |
| `H`    | `unsigned short`     | integer            | 2      |
| `i`    | `int`                | integer            | 4      |
| `I`    | `unsigned int`       | integer or long    | 4      |
| `l`    | `long`               | integer            | 4      |
| `L`    | `unsigned long`      | long               | 4      |
| `q`    | `long long`          | long               | 8      |
| `Q`    | `unsigned long long` | long               | 8      |
| `f`    | `float`              | float              | 4      |
| `d`    | `double`             | float              | 8      |
| `s`    | `char[]`             | string             | 1      |
| `p`    | `char[]`             | string             | 1      |
| `P`    | `void *`             | long               |        |

Note 1: `q` and `Q` are only meaningful when the machine supports 64-bit operations.

Note 2: A number can be placed before each format to indicate the count.

Note 3: The `s` format represents a string of a certain length. 4s represents a string of length 4, but `p` represents a pascal string.

Note 4: `P` converts a pointer, and its length is related to the machine word length.

Note 5: `P`  is used to represent pointer types. For Quectel modules, it occupies 4 bytes.



### 1.2 ustruct Alignment

To exchange data with C structures, it is necessary to consider that some C or C++ compilers use byte alignment, and that it is usually a 32-bit system (unit in 4 bytes). Therefore, ustruct converts the data according to the local machine byte order. The alignment can be changed by the first character in the format. The definitions are as follows:

| Character | Byte order    | Size | Alignment       |
| :-------- | :------------ | :--- | :-------------- |
| @(default)   | Native          | Native | Native, aligned to 4 bytes  |
| =         | Native          | Standard | None, aligned to original size |
| <         | Little-endian          | Standard | None, aligned to original size |
| >         | Big-endian          | Standard | None, aligned to original size |
| !         | Network (big-endian) | Standard | None, aligned to original size |


### 1.3 pack and unpack Functions

The *pack* function packs a sequence of Python values into a byte string according to the specified format (fmt), while the *unpack* function unpacks a byte string into a tuple according to the specified format string (fmt).

Here is an example of using the *pack* and *unpack* functions:

```python
import ustruct

buf = ustruct.pack('4sI', b'MPYN', 12345)
print(buf)  # Output b'MPYN\x39\x30\x01\x00'

s, i = ustruct.unpack('4sI', buf)
print(s, i)  # Output b'MPYN' 12345
```

In this example, '4sI' represents packing and unpacking with a 4-byte string and an integer.



### 1.4 calcsize Function

The *calcsize* function returns the length of the byte string required for the specified format (fmt).

Here is an example of using the *calcsize* function:

```python
import ustruct

size = ustruct.calcsize('4sI')
print(size)  # Output 8
```

In this example, '4sI' represents packing with a 4-byte string and an integer, requiring a length of 8 bytes.



### 1.5 pack_into Function

The *pack_into* function packs a sequence of Python values into a specified buffer (buffer) at a specified offset (offset) according to the specified format (fmt).

Here is an example of using the *pack_into* function:

```python
import ustruct

buf = bytearray(8)
ustruct.pack_into('>hhl', buf, 0, 32767, -12345, 123456789)
print(buf)  # Output: b'\x7f\xff\xcf\xc7\x80\x8d\x05\xcb'
```

In this example, '>hhl' indicates the use of big-endian byte order to pack a 16-bit integer, a 32-bit integer, and a 32-bit signed integer into a byte string, and place them at offset 0 of the buffer.

### 1.6 unpack_from function

The *unpack_from* function starts processing the binary data and unpacks it into a tuple from the specified offset of a format string (fmt), and a  byte string.

Here is an example of using the *unpack_from* function:

```python
import ustruct

data = b'\x01\x02\x03\x04\x05\x06\x07\x08'
a, b = ustruct.unpack_from('HH', data, 2)
print(a, b)  # Output 0x0302 0x0405
```

In this example, 'HH' indicates the unpacking of two 16-bit unsigned integers. The *unpack_from* function unpacks the data starting from offset 2 in the byte string into two integers based on a format string.



### 1.7 Endianness Testing

MicroPython supports both big-endian and little-endian byte order when dealing with binary data. The byte order can be specified by the format string as either big-endian ('>') or little-endian ('<') for packing and unpacking.

Here is an example of using both big-endian and little-endian byte order:

```python
import ustruct

buf = ustruct.pack('>Hl', 32767, 123456789)
print(buf)  # Output b'\x7f\xff\x05\xcd\x15\xcd\x5b\x07'

s, i = ustruct.unpack('<Hl', buf)
print(s, i)  # Output 258 123456789

```

In this example, '>Hl' indicates the use of big-endian byte order to pack a 16-bit integer and a 32-bit signed integer into a byte string, while '<Hl' indicates the use of little-endian byte order to unpack the same byte string into a 16-bit integer and a 32-bit signed integer.



### 1.8 Summary

The ustruct module is used for packing, unpacking, encoding and decoding binary data, supporting both big-endian and little-endian byte order.

Its main functions include *pack*, *unpack*, *calcsize*, *pack_into* and *unpack_from*. 

> The *pack*  function packs a sequence of Python values into a byte string according to the specified format string.
>
> The *unpack* function unpacks a byte string into a tuple
>
> The *calcsize* function returns the length required for a byte string with the specified format
>
> The *pack_into* function packs a sequence of Python values into a specified buffer according to the specified format,
>
> The *unpack_from* function unpacks a byte string starting from a specified offset into a tuple

By using these functions, it is convenient to perform packing, unpacking, encoding and decoding operations on binary data.

## ubinascii

The ubinascii module is used for encoding and decoding binary data. It provides various encoding and decoding methods, such as hexadecimal encoding and decoding, Base64 encoding and decoding, etc.



### 2.1 hexlify and unhexlify Functions

The *hexlify* function encodes a byte string into a hexadecimal string, and the *unhexlify* function decodes a hexadecimal representation string into a byte string.

Here is an example of using the *hexlify* and *unhexlify* functions:

```python
import ubinascii

data = b'\x01\x02\x03\x04\x05\x06\x07\x08'
hexstr = ubinascii.hexlify(data)
print(hexstr)  # Output b'0102030405060708'

bytearr = ubinascii.unhexlify(hexstr)
print(bytearr)  # Output b'\x01\x02\x03\x04\x05\x06\x07\x08'

```

In this example, the *hexlify* function encodes the byte string b'\x01\x02\x03\x04\x05\x06\x07\x08' into the hexadecimal string b'0102030405060708', and the *unhexlify* function decodes the hexadecimal string into the byte string b'\x01\x02\x03\x04\x05\x06\x07\x08'.



### 2.2 b2a_base64 and a2b_base64 Functions

The *b2a_base64* function encodes a byte string into a Base64 string, and the *a2b_base64* function decodes a Base64 string into a byte string.

Here is an example of using the *b2a_base64* and *a2b_base64*  functions:

```python
import ubinascii

data = b'\x01\x02\x03\x04\x05\x06\x07\x08'
base64str = ubinascii.b2a_base64(data)
print(base64str)  # Output b'AQIDBAUGBwg='

bytearr = ubinascii.a2b_base64(base64str)
print(bytearr)  # Output b'\x01\x02\x03\x04\x05\x06\x07\x08'

```

In this example, the  *b2a_base64* function encodes the byte string b'\x01\x02\x03\x04\x05\x06\x07\x08' into the Base64 string b'AQIDBAUGBwg=', and the *a2b_base64*  function decodes the Base64 string into the byte string b'\x01\x02\x03\x04\x05\x06\x07\x08'.



### 2.3 Summary

The ubinascii module is used for encoding and decoding binary data.

Its main functions include *hexlify*, *unhexlify*, *b2a_base64* and *a2b_base64*.

>The *hexlify* function encodes a byte string into a hexadecimal string. 
>
>The *unhexlify* function decodes a hexadecimal string into a byte string. 
>
>The *b2a_base64* function encodes a byte string into a Base64  string. 
>
>The *a2b_base64* function decodes a Base64 string into a byte string.

By using these functions, it is convenient to perform encoding and decoding operations on binary data.