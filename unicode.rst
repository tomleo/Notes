Every letter is assigned a magic number written like this::

    U+0639

The string "Hello" == `U+0048 U+0065 U+006C U+006C U+006F`, this can encoded
like so::

    48 00 65 00 6C 00 6C 00 6F 00

    or

    00 48 00 65 00 6C 00 6C 00 6F

    or

    FE 48 00 65 00 6C 00 6C 00 6F

    or

    FF 48 00 65 00 6C 00 6C 00 6F

The FE and FF are Unicode byte order marks so the code points can be sored in
high-endian or low-endian mode.

UTF-8 is a system for storing unicode code points. Every code point from 0-127
is stored in a single byte. Code points 128 and above are stored using 2-6
bytes.

UCS-2
    Store  in two bytes

UTF-16
    16 bits / two bytes, low/high endian are specified by the programmer

ISO 8859-1
    Latin 1 encoding


If there's no equivalent for the Unicode code point you're trying to represent
in the encoding you're trying to represent it in, you usually get a little
question mark: ? or, if you're really good, a box. Which did you get? -> �

meta tag really has to be the very first thing in the <head> section because as
soon as the web browser sees this tag it's going to stop parsing the page and
start over after reinterpreting the whole page using the encoding you
specified.

ASCII::

    char        I
    hex         \x49
    decimal     73

s.decode(encoding)::

    <type 'str'> to <type 'unicode'>

u.encode(encoding)::

    <type 'unicode'> to <type 'str'>

Unicode::

    letter  | unicode point | utf-8
    ć       | U+0107        | \xc4\x87


BOM (byte order mark) can be 2, 3, or 4 bytes long

utf16 BOM = '\xff\xfeI\x00'


