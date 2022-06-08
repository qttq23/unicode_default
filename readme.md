
# About Unicode, ascii, utf-8/16, BOM

### ASCII
127 characters, each is assigned a `Code Point` and represented by 7 bits. Bit's value is equal to the corresponding code point.  
(view `Control code chart` at: https://en.wikipedia.org/wiki/ASCII)

### ANSI
it is an old standard of Windows. it uses 8 bits to encode code points. it's compatible with ASCII but not vice versa.

### Unicode
Unicode is a standard that defines UTF-8, UTF-16, UTF-32.  
Vendors like OS, programming languages implement Unicode encoder/decoder comforming to Unicode standard.
(view `Code planes and blocks` at: https://en.wikipedia.org/wiki/Unicode)

### UTF-8
utf-8 is an extension of ascii. utf-8 uses 1 byte to hold first 127 characters (ascii). and use 2 or 3 or 4 to hold other characters.

if a character can be represented by 1 byte, utf-8 uses: `0xxxxxxx` to encode the code point of that character. (0 at the start is mandatory)  
if a character needs 2 bytes, utf-8 uses: `110xxxxx 10xxxxxx`. the code point is splited into 2 parts, each part is prepended with `110` and `10`.
and the same manner with 3 or 4-byte long characters.
(view `Encoding` and `Examples` at: https://en.wikipedia.org/wiki/UTF-8)

Remember that the encoded value is not neccessarily equal to the corressponding code point.
only the first 127 characters is equal numerically to its code point. later characters will change value after encoding.
The purpose of putting `0` or `10` at the start of encoded value is to distinguish which character needs how many bytes. 
for example, when you read a byte starts with `0`, you know the character is 1 byte-long. 
if you encounter `110` at start of a byte, you know that the character is 2-byte long and you need to read 1 more byte to interprete the character. 
and so on for 3 or 4 bytes.

BOM (byte order mark) for utf-8 is: 0xEF,0xBB,0xBF. most of the time, utf-8 doesn't need these BOMs.

### UTF-16
utf-16 uses at least 2 bytes (16 bits) to encode code points. so utf-16 is not compatible with ASCII. 
if a character cannot be encoded using 2 bytes, utf-16 uses two 16-bit code units (4 bytes) which is called `surrogate pair`.

utf-16 uses 2 bytes to encode code points from 0 to FFFF (which is larger than ascii range). the value is equal numerically to its corresponding code point.
utf-16 uses surrogate pair to encode other code points (from 0x10000 to 0x10FFFF). It first subtracts the code point by 0x10000 to make it less than 0xFFFFF.
Then, the first 10 bit is added to `0xD800` to make the first part (16 bits) of the `surrogate pair` and last 10 bit is added to `0xDC00` to make the last part (16 bits).
(view `Description` and `Examples` at: https://en.wikipedia.org/wiki/UTF-16)

because of the present of two 16-bit units, it raises the issue of endianness. utf-16 can use 0xFEFF (BOM or byte order mark) to distinguish endianness.
if you encounter the sequence of byte starts with 0xFEFF, you know you are decoding utf-16 with same endianness as encoding.
if you encounter the sequence of byte starts with 0xFFFE, you know you are on different endianness and you should swap the order of 16-bit unit in surrogate pair.
 

### samples:
use `Hex Editor` to view `abc_utf8.txt` and `abc_utf16_bigendian.txt`. (for short, view screenshots)
use `notepad` to save text file as utf-8 or utf-16.


