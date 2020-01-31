#
C
#


Chapter 1
*********

Section 1.1
===========

A program consists of functions and variables
:code:`#include <stdio.h>` includes the standard input output library


Section 1.2
===========

C types

+---------+---------------------------------+
|   char  |  character                      |
+---------+---------------------------------+
|   int   |  integer                        |
+---------+---------------------------------+
|   short |  short integer                  |
+---------+---------------------------------+
|   long  |  long integer                   |
+---------+---------------------------------+
|   float |  single-precision floating point|
+---------+---------------------------------+
|   double|  double-precision floating point|
+---------+---------------------------------+

Integer division truncates!!


Section 1.4
===========

Symbolic constants:

* #define NAME replacement list
* They are conventionally written in upper case, to be distinguished by variables. 
* #define is not terminated with a column.

Section 1.5
===========

text stream: sequence of characters divided into lines
:code:`c = getchar();` gets a character from input
:code:`= putchar(c);`  prints a character to the output
:code:`EOF` on the mac is ctrl-D

In :code:`c = getchar()` we define c to be int, because EOF is outside the char 
range (0 to 255, or -127 to 128) and char wont be able to hold it. This is 
what the book says, but in my implementation, setting c as char works as 
well.

**Very important**: In c, any expression, such as :code:`c = getchar();` has a value, and is the value 
of the left hand after the assignment. This is what allows us to write
:code:`while ( (c = getchar()) != EOF)`

The parentheses arount the assignment are necessary as :code:`!=` has higher 
precedence than the assignment :code:`(=)`

:code:`int fgetc(FILE *stream)` reads a character from a file and returns :code:`EOF` when 
finishes.

:code:`char *fgets(char *str, int n, FILE *stream)` reads a line from the specified 
stream and stores it to the string pointed by str. It stops when either 
(n-1) characters are read, the newline character is read, or the end-of-file
is reached, which ever comes first. The pointer returned by fgets is the 
pointer to the beginning of the line.

A character constant, is a character written between single quotes and 
represents an integer value equal to the numerical value of the character in
the machine's character set.  So, :code:`c - '3'`, where :code:`c` is an int will subtract 
51 (the ascii code for 3) from :code:`c`.

Section 1.6
===========

Section 1.7
===========

* function declaration (function prototype)
* function definition (where the actual implementation of the function is 
given)

Section 1.8
===========

* Call by value: a local copy is passed
* Call by reference: the original variable is passed (can be modified)

Section 1.9
===========

Every string is terminated by :code:`'\0'`

The minimum length of an empty line is 1 (for the :code:`'\n'` character)

the string hello with a new line, requires 7 chars

.. code-block:: 

    h  e  l  l  o  \n \0
    
    1  2  3  4  5  6  7

Files, :code:`\n`  and :code:`EOF`
* A file is always terminated by :code:`EOF`
* An empty file will have only one character, the :code:`EOF`
* Every line in a file (even the last one) is terminated by :code:`\n`.
* A file with a single empty line will have 2 characters: :code:`\n` and :code:`EOF`.
* An :code:`EOF` will always be followed by :code:`\n`, unless it's an empty file.

Section 1.10
============

* Automatic variables: variables that dissappear when the function is exited. 
* Static variables:    variables that persist between function calls
* External variables:  variables common to all functions. These variables are defined
    outside functions. In order to be used within a function they have to be declared with the keyword :code:`'extern'`. This is compulsory if the variable is defined in another file, and optional if defined in the same file and before the function where it is to be used. 


Chapter 2
*********

Declarations, Operators, Expressions
====================================

Don't start variables with a :code:`_`

Variable names are lower case (convention)
    
Headers :code:`<limits.h>` and :code:`<float.h>` contain symbolic constants for all sizes
    
Data types:
-----------

* char
* int 
* float
* double
* qualifiers: short, long
* signed or unsigned can be applied to chars and ints
    
Constants:
----------

* long constants: terminal l or L
* unsigned constants: terminal u or U
* doubles: dot (.) or exponent (e.g. 1e-2)
* floats: f or F
* octal: A leading 0 on an integer constant
* hexadecimal: A leading 0x

A character constant is an integer, written as one character within single quotes

Constant expression
-------------------

* :code:`#define MAXLINE 1000`

String constant (or literal)

* :code:`"I am a string"`
* :code:`"Watch the double quotes!"`
* Strings must be terminated with a :code:`'\0'`

Enumerations
------------

* enum boolean :code:`{NO, YES};`
* enum escapes :code:`{BELL='\a', BACKSPACE='\b'};`
* enum months :code:`{ JAN = 1, FEB, MAR };` /* Feb will be 2 MAR 3 etc*/

Relational and Logical operators
--------------------------------

* :code:`||` and :code:`&&` are evaluated left to right and evaluation stops as soon as the truth or falsehood is known.

Cast
----

* :code:`int n = 3;`
* :code:`sqrt((double) n);`
    n is cast as double before passed to the sqrt function

Unary operator
--------------

* :code:`~077` inverts all the bits of the number


Bit Shifting
------------

* Left shifting :code:`<<` will fill empty positions with zeros
* Right shifting :code:`>>` will fill the positions with zeros for unsigned 
* quantities
* For signed quantities will fill with bit signs on some machines 
* (arithmetic shift) and with zeros on others (logical shift).

One complement
--------------

* inverting all the bits (achieved with the unary operator (~)

Two complement
--------------

* invert all the bits and add 1. 
* Suppose a 1 byte signed number (8 bits e.g. a char). The number range is -2^(8-1) to 2^(8-1) -1, i.e. -128 to 127. A negative number is given by the complement 2 of the positive. E.g. 1: 00000001, -1: 11111111 (invert bits and add 1).  127: 01111111, -127: 10000001.  The only negative number that does not have a positive in this range is -128 its binary representation is -128: 10000000. Note that the 2-complement of -128 is itself!

Ternary operator
----------------

* :code:`expr1 ? expr2 : expr3`
      Returns expr2 if expr1 is true or expr3 otherwise.

Chapter 3
*********

Statements are terminated by colons.

:code:`{}` define compound statements, or blocks.

Chapter 4
*********

Variables defined within a function or a scope are internal variables

Variables defined outside of any scope are external. These are visible to all 
functions that follow in that file.

An internal variable that is declared static persists between function calls 
and keeps its value.

An external variable (or any function, which is by definition an external 
object) 
which is declared static is hidden (not accessible) by anything outside the file
within which the variable of function is defined

To use an external variable defined in file1 from file2, the variable must be 
declared in file2 as 
:code:`extern external_variable_name;`

Chapter 5
*********

c does not support pass by reference in the way c++ does. Pass by reference in 

c happens by passing the addresses of the variables we want to change. 

Pointers and arrays are very similar. One difference is that a pointer is a 
variable but an array is not. So :code:`pa = a` and :code:`pa++` are legal (:code:`pa` is a pointer). 
However, :code:`a = pa` and :code:`a++` are not (:code:`a` is an array).
On the other hand, when an array is passed as an argument to a function, 
within that function the array is converted to a pointer (variable), and 
:code:`a = pa` and :code:`a++` are then legal.

Pointer differences should be stored in :code:`ptrdiff_t` or preferably in :code:`size_t` 
types. 

While storing differences in integers is legal, integers may not be able to hold
large differences in memory. :code:`ptrdiff_t` and :code:`size_t` are defined in :code:`<stddef.h>`

**Pointer arrays vs multidimensional arrays:** Although char :code:`a[10][20];` and char :code:`*b[10];` have some similarities, :code:`a` is a true 200 char array and the respective memory has been set aside for it. :code:`b` on the other hand, is just an array of 10 pointers, and the second dimension needs to be initialised. On the other hand, the rows of :code:`b` can have variable dimension, but the rows of :code:`a` have all 20 elements.

Chapter 6
*********

Operator precedence

* :code:`x = *p++` stores :code:`*p` in x and increases :code:`p` by one

* :code:`x = *(p++)` is the same as :code:`x = *p++`

* :code:`x = (*p)++` stores :code:`*p` in x and whatever was in :code:`*p` is increased by one. E.g.  if :code:`*p = 20`, then :code:`x = (*p)++` will store 20 in :code:`x`, but :code:`*p` will become :code:`21`!

Size:

* :code:`sizeof <variable>` returns the size of variable in bytes.
* :code:`sizeof(type)`      returns the size of a type, e.g. sizeof(int)
* :code:`sizeof` returns the type :code:`size_t`, which is a long integer.

File I/O
********
* :code:`open`
* :code:`close`
* :code:`getc` (get character)
* :code:`putc` (put character)
* :code:`gets` (get line)
* :code:`puts` (put line)
* :code:`stdin` and stdout are pointers to the standard input and output
* :code:`stderr` is the standard error output
* :code:`system(char *s)` executes a system command


