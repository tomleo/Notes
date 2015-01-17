C++ Notes
=========

Initialization of Vars
----------------------

.. code-block:: cpp

    type identifier = initial_value ;		int a = 0;
    type identifier (initial_value) ;		int a (0);

Numerical Constants
-------------------

By default constants are type int::

    75         // decimal
    0113       // octal
    0x4b       // hexadecimal
    75         // int
    75u        // unsigned int
    75l        // long
    75ul       // unsigned long 

Floating Point Numbers::

    75         // int
    75u        // unsigned int
    75l        // long
    75ul       // unsigned long
    3.14159L   // long double
    6.02e23f   // float

Strings
-------

.. code-block:: cpp

    #include <string>
    string mystring = "This is a string";	//note string is lowercase unlike Java

Double quoted strings (") are literal constants whose type is in fact a
null-terminated array of characters. String literals enclosed between double
quotes always have a null character ('\0') automatically appended at the end.

Character and string literals
-----------------------------

you can express any character by its numerical ASCII code by writing a
backslash character (\) followed by the ASCII code expressed as an octal
(base-8) or hexadecimal (base-16) number::

    \x4A				                                //J
    "string expressed in \		                        // multi-line string literal
    two lines"
    "this forms" "a single" "string" "of characters"	// String concat
    L"This is a wide character string"	                // Wide character

Assignment
----------

.. code-block:: cpp

    a = 2 + (b = 5);	//is equal too:  b = 5;    a = 2 + b;
    a = b = c = 5;		//Will assign 5 to a, b, and c
    price *= units + 1;	//equal too:  price = price * (units + 1);

Conditional operator ?
----------------------

*condition ? result1 : result2*

.. code-block:: cpp

    7==5 ? 4 : 3     //returns 3, since 7 is not equal to 5.
    7==5+2 ? 4 : 3   //returns 4, since 7 is equal to 5+2.
    5>3 ? a : b      //returns the value of a, since 5 is greater than 3.
    a>b ? a : b      //returns whichever is greater, a or b.

Operations
----------

Typecasting
```````````

.. code-block:: cpp

    i = (int) float_var; //normal typecast float -> var
    i = int ( float_var ); //alternate way to typecast float -> var

sizeof()
````````

.. code-block:: cpp

    a = sizeof (char); //returns size in bytes of type or object

Precedence
``````````

TODO: Finish this
