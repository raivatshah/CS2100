# CS2100 (AY19/20 Sem 1) Revision Notes for Mid Term
#### Prepared by Raivat Shah 

## Lecture 2 - Overview of C

At the lowest level, hardwre components work on 2 states: On(1) & Off(0)/ So data & instructions must be in binary. 

**Different Languages**

Natural Language (Easiest to express) &rarr; High-Level Language (eg C/Python) &rarr; Assembly Language (eg MIPS) &rarr; Machine Code (eg 0xAAAABBBB, closest to hardware)

**Translater**

Source Lang &rarr; Translater &rarr; Target Lang

If Source Lang = High-level lang, Translator = Compiler. 

**Life of a Program**

*Compiled*: Editor -> Compiler -> Assembler -> Linker -> OS
Entire prog compiled before executation into an executable. Just in Case execution. Eg for C/C++/Swift

*Interpreted*: Editor -> Interpretor
Compilation on the "fly." Executed statement by statement at Run Time. Just-in-Time execution. Eg for Python/JS. 

*On a Virtual Machine*: Editor -> Compiler -> VM -> OS
Same as *Compiled* but compiled into an intermediate executable that runs on a VM. 

### The C Language - We follow ANSI C (C90)

General Purpose PL developed in 1972 by Dennis Richie for use with UNIX Operating System. Provides only a min core mechanism. Exposes underlying machine model while still being portable. Assumes Programmer Knows best. Procedural in Nature. Very efficient in execution speed and memory footprint. 

Sample C program:

```c
#include <stdio.h>

int main()
{
    printf("Hello World\n");
    return 0;
}
```
Notes:

1. All code resides within a function. So a `.c` program is just a bunch of funcs.
2. Program starting point is always the `main` function, which always returns `0`.
3. `#include<stdio.h>` is a pre-processor directive. Means add the file `stdio.h` for compilation. Now can use code from that file, eg `printf`. Can define global constants using `#` also. 
4. C is case sensitive and a statement is equivalent to a step in an algorithm. 

**C Variables**

* Each variable is stored in the memory (RAM), a long 1D array. 
* Must be declared before use & data type specified at declaration. Data type decides type of value, size of value (no of bytes), & range of values (depends upon size)
* Do not have an initial value.

| Data Type     |  32-bit processor | 64-bit processor  |
| ------------- |:-------------:| -----:|
| `int`     | 4 bytes | 8 bytes |
| `float`    | 4 bytes     |   8 bytes |
| `double` | 8 bytes      |    16 bytes |
| `char` | 1 byte     |   1 byte |

*Note* 1 `byte` = 8 `bits`. 1 `byte` can represent upto 2^numBits - 1 = 2^8 -1 = 255in base 10. So, for 4 byte `int`, the range is from [-2^31 to 2^31 - 1]. 

Range Limitation:  
```c
#include <stdio.h>

int main()
{
    int i = 2147483647; //2^31 - 1. Max possible value for int in c. 
    while(i > 0) {
        printf("%d", &i); //output will be 2147483647.
        i++;
    } // loop will only run once.

    printf("%d", &i); //output will be -2147483647. 
}
```
After hitting the maximum number, C variables will "go back" to the 0, something like a clock or the `modulo` operator in math. Note that need to *dereference* variable with a pointer to refer to it.

Accuracy Limitation: 

**C Type Conversion**

*Hierarchy* `char` -> `int` -> `float` -> `double`

Widening: going to right from left. No loss of information. Eg 1 to 1.00 

Narrowing: going to left from right. Loss of info. Eg 1.7 to 1. 

Implicit: Performed automatically based on defined rules. 

Explicit: Programmer decides data type change. 

*Rules for Implicit Conversion*

Consider `A op B -> C` (Arithmetic)

* if A and B same type, then C also same type. Eg 5 + 6 = 11, 5 / 2 = 2
* Else, lower data type promoted to higher (widening) and C is of the higher data type. Eg 3.0 * 4 = 12.0, 5 / 2.0 = 2.5. 

Consider `A = B` (Assignment)

B will be widened/narrowed based on data type of A. Think of this as `B` going into `A` so it must abide by `A`'s rules. 

*Explicit Casting*

Use (`data type`) + variable to convert explicitly. Eg (`double`) 2 -> 2.0

## Lecture 3 - Data Representation & Number Systems

**Different bases and conversions**

Base 10 is only a way of representing powers of 10. Eg: 2915 = 5 x 10^0 + 1 x 10^1 + 9 x 10^2 + 2 x 10^3. 

We can have different bases too: 

Base 10 -> Denary, Base 2 -> Binary, Base 8 -> Octal, Base 16 -> Hexadecimal. 

*Converting base R to base 10:*

(572) base 8 = 2 x 8^0 + 7 x 8^1 + 5 x 8^2  = 378 base 10. 

For simple arithmetic in base R, use upto max digit allowed and then carry. Eg:

0b10 + 0b11 = 0b101 = 5 (base 10). (As 1+1 = 2 but 2 is not a digit in binary, carry the one over.)

*Converting base 10 to base R:*

Repeated division by powers of R less than the number in base 10 until remainder is 0. Eg

(89) base 10 to binary:

89 / 64 = 1 remainder 25
25 / 32 = 0 remainder 25 
25 / 16 = 1 remainder 9
9 / 8   = 1 remainder 1
1 / 4   = 0 remainder 1
1 / 2   = 0 remainder 1
1 / 1   = 1 remainder 0

Therefore, (89) base 10 = (1011001) base 2. 

*Base K to Base J*, use 10 as a "bridge." Shortcut exists between base 2 to base 8/base 16. 

For Base 2 and Base 8: 
Every 3 digits of binary represent 1 digit of Octoal. Eg Binary 111 is Octal 7. Binary (00)1000001 in Binary is 101 in Octal. In general, group bits of Binary into triplets for the conversion. 

For Base 2 and Base 16:
Every 4 digits of binary represent 1 digit of Hexadecimal. Eg Binary 1111 is Hexadecimal F (15). Binary (0)1000001 in Binary is 41 in Hexadecimal. In general, group bits of Binary into quadruplets for the conversion. 

**Data Representation on Computers** 

All info (image, video, application, etc) on a computer is stored as binary values. 

Common Storage Units:
* Bit (Binary Digit) is a single 0 or 1.
* Byte - 1 Byte = 8 bits. (Usually smallest accessible unit)
* Word - 4 bytes/8 bytes (platform dependent). Double/Half world also possible. 

N bits can represent upto 2^N different values eg 2 bits can represent 2^2 = 4, 3 bits can represent 2^3 = 8, and so on...

To represent M values, need Math.Ceil(log base 2 of M)

Data Types in Programming Languages Map to Underlying Storage Units. We explore their representation using limited bits. 

**Integers**

Unsigned Numbers - Only non-negative values. Easily represented using simple binary digits.

Signed Numbers - Negative & Positive values. Need a representation. 

##### Common Representations 

#### Sign-and-Magnitude

Sign represented by a 'sign-bit' - the most significant bit. 0 for positive, 1 for negative. 

| sign |   | digit | | digit | | digit | | digit | | digit | | digit | | digit |

Representation range from -127 to 127 (inclusive in base 10)

For n-bit sign-and-magnitude representation, range of values is [-2^n-1, 2^n-1]. Only one zero. 

Sign-and-magnitude not very good with arithmetic operations. 

#### Complements

Complements are very good for subtraction and logical manipulations. 

In *1s complement*, positive num X of binary stays X. However, -X becomes 2^n - X - 1. To calculate, invert all bits. MSB indicates sign 

For n-bit 1s complement, range is [-2^n-1, 2^n-1]. There are 2 zeros (positive & negative). 

For addition: `A + B`. Perform binary addition. If carry of MSB, add 1 to the result. Check for overflow (when sign of A & B is opposite)

For subtraction: `A - B = A + (-B)`. Take 1s complement of B and add that to A.  

In *2s complement*, positive num X of binary stays X. However, -X becomes 2^n - X. To calculate, invert all bits (1s complement) and add 1 to the result. MSB indicates sign.

For n-bit 2s complement, range of values is [-2^n-1, 2^n-1]. Only one zero. 

For addition: `A + B`. Perform binary addition. Ignore carry of MSB. Check for overflow (when carry in is not equal to carry out or when signs are different)

For subtraction: `A - B = A + (-B)`. Take 2s complement of B and add that to A.  

Can extend idea of complement to fraction. Eg 0101.01's 1s complement is 1010.10. 
0101.01's 2s complement is 1010.11. 

"Find complement of a number" -> "Find the negated value in that complement system"

Eg find the 1's complement of 0110 is 1001. 

Eg find the 1's complement of 101 [8 bit], is 100000000 - 101 - 1 = 11111010.

#### Excess-K Representation

Use a simple translation to represent a number X. Add K to X, then represent result in binary. K = bias/offset. There is no standard for offset binary, but most often the offset K for an n-bit binary word is K = 2n−1. Primarily used for the exponent of floating-point numbers.

Eg 7 in Excess-8 is 7+8 = 15 = 1111 (base 2)

**Floating Point Numbers**

Binary point may be at any pre-fixed location. Allow us to represent very large or very small values. 

Commonly Used Format: IEEE 754 

For Single-precision floating point number on a 32-bit platform:

1 bit for Sign - 8 bits for Exponent (Excess-127) - 23 Bits for Fraction/Mantissa. Normalized to 1.X and take X only. Example:

(-39.625) -> (-100111.101) base 2 -> (-1.0011101) x 2^5 (normalization)

Therefore, sign = 1 (negative), exponent = (5 + 127 = 132 -> 1000 0100), fraction = (0011101). 

**Characters**

Includes printable (eg A, B, ...) & non-printable (eg NULL, bell, tab, return)

Old: ASCII 8 bit sequence for representing 255 characters
New: UNICODE. Encoding schemes for backward compatibility. 

## Lecture 4 - Function & Pointer

**C Functions**

Large program should be modular. Function allows better maintenance and reusability. Sample func header:

`int sum(int x, int y)` Includes input and its type, return type and the name. 

Function can return atmost 1 result directly. Variables declared in function is only alive and visible in that function. 

Actual arguments in a function call have a 1-to-1 correspondence with the formal parameters declarated in function header. Eg `sum(3, 4)` implies `x = 3, y = 4` in the above example. 

Single value returned by the function replaces the function call and can be userd in normal operations and assignment. 

**C Pointers**

A variable in C is essentially a location in memory that holds a value. Each location has a unique address and locations are arranged consecutively (recall memory is a long 1D array). Compiler handles laying out of variables. 

`int x = 123` -> name = x, content = 123, address = 0x4096034A (example)

A pointer variable stores the address of a memory location and is declared with an `*`. Data type of a pointer variable is the data type of the value of the original variable. Pointer variable always store an `int` address. 

`int *ptr = &x` -> ptr now holds 0x4096034A. `&` gives address of a variable. 

Pointer variable manipulation works in the same way as normal variables. Eg:

```c
int x;
int *ptr, *ptr2;
ptr = &x;
ptr2 = ptr; //ptr2 also points to int x now. 
```

**C variable Passing**

Two ways to pass a param into a func:
* Pass by value (cannot mutate outside function call. Simple data types and structures are passed by value).
* Pass by address/pointer (can mutate outside function call, that's why use it for `scanf`. Requires caller to pass address using `&`. Arrays are passed by address. 

**C Library**

A collection of functions, constant definitions etc. Prototypes (similar to function headers) are provided in a header file (`XXX.h`). User only needs to `#include <XXX.h>` to use it. C Standard Library - set of libraries specified by C specification. 

## Lecture 5 - Arrays 

Just like list in Python. Arrays are immutable in C but with mutable values. Syntax: 

Declaration: `datatype identifier[size]`

Declaration with initialization: `datatype identifier[size] = { init_list }` init_list can shorter than size (rest get `0` by default). Use `{ 0 }` to initialize all array items to `0`.

`identifier` now refers to the whole array. To access individual items, use array index starting at `0`. Eg `myArray[0]` will give first element. Each individual array item is simply a variable of the same type. 

Array name is the same as the address of the 0th element. Eg:

```c
int ia[3] = {3, 5, 7};
int *ptr;

ptr = ia; // no need of & as ia stores address of the first element already
prt[1] = 333; //changes 5 to 333

ptr = &(ia[1]); // now stores address of 333 
ptr[1] = 4444; // now changes 7 to 4444. 

//at the end, ia = {3, 333, 4444}
```

Array can be a function parameter but not a return data type. Must also have the size of the array as another input parameter. Eg: `int sumUp(int a[], int size)`. To pass in array, can simply use its name (`&` not required for mutation).

*Character & String*

In C, a String is simply an array of characters. `#include <string.h>` when using Strings. 

A `char` is encoded as a small 1 byte integer. Declared with single quotes `''`. Can be manipulated as a number:

```c
char a = 'a'
a = a + 3 // 'a' is now d. 
```

It is a String only when terminated by a String Terminated (special `\0` character). C String is a special case of character array. String constants enclosed by double quotes `""`. String terminator is counted in the array size. Can be manipulated as a whole, use `%c` placeholder. 

Read a single word using `scanf`. To read sentence (whole line), use: 

`char* fgets(char str[]. int n, FILE *stream)`. Read upto n-1 characters or until newline. Result stored in `str`.

Use `stdin` as stream parameter for taking input from keyboard. 

`int strlen(char str[])` to count number of chars excluding terminator.

`char* strcopy(char dest[] char source[])` to copy from source to destination. 

`char* strcat(char dest[], char source[])` to append source string to end of dest string.

`int strcmp(char s1[], char s2[])` to compare ASCII value of each char in `s1` with `s2` until end of string OR `s1[i] != s2[i]`. Returns `0` if strings are the same, `<0` if `s1[i] < s2[i]`, `>0 if s1[i] > s2[i]`. 

## Lecture 6 - Structure

Many entities consist of multiple pieces of info: 

* Fraction with numerator and denominator 
* Cuboid with length, breadth and height
* Student info: name, age, number, etc

Difficult to maintain different info as different variables. Use `structure` as a logical container for these values. Just like a class in OOP. Arrays store homogeneous data, struc stores hetrogeneous data. Structure is passed by address commonly to avoid memory and time wastage and to allow function to mutate struc's value. 

Eg:

```c
struct Fraction{ //structure declaration. Essentially a new data type.
    int num; //fields are stored in adjacent locations in memory. 
    int den;
}; //this is just a declaration. No actual variable is allocated.

int main{

    struct Fraction frac1 = { 0 } // use new data type to declare a Fraction structure.
    frac1.num = 9// change num to 9
    frac1.den = 10 //change den to 10

    return 0 //always return 0.
}
```
Dereferencing a structure: `(&fptr).num is the same as ftpr->num`. Structure can have structure as a field. Can have array of structures or have array as field. 

## Lecture 7 - MIPS Introduction

How C files are compiled: `gcc -v hello.c` 

How C files are executed: `./hello`
e

**Assembly Code**

Symbolic version of machine code, human readable. Assembler translates further from assembly langugae to machine code. Assembly can provide pseudo-instructions as syntactic sugar for complext instructions. When considering performance, only real instructions are counted. 

*Instruction Set Architecture*: A subpart of computer architecture that is related to programming, as seen by the programmer and compiler. Exponses the capabilities of the underlying processor as a set of well defined instructions (something like an API for the processor). Thus, ISA is an interface between software and hardware, and serves as an abstraction barrier which allows freedom in hardware implementation. 

Examples of ISA: x86-64 (mostly used in PCs), MIPS (used for study purposes), ARM (used in embedded system chip for mobile devices). If a chip follows a certain ISA, it has all the functions guaranteed by the ISA. Program compiled for ISA can run on any of the "family of chips" under the ISA. 

**Program Walkthrough Notes**

Two major components in a computer: Processor & Memory. 

Stored-Memory Concept: instructions (in assembly code) & data reside in Memory, only transferred to Processor during execution. Instructions decoded in the processor. 

Load-Store Model: Memory access is slow. To avoid frequent access, *registers* (`$r0`, `$r1`, etc) in processor provide temporary storage during execution. Arithmetic operations directly work on registers. 

Major Types of Assembly Instruction: 
* Memory: Move values between memory and register
* Calculation: Arithmetic and other operations
* Control flow: Changes the sequential execution. 

**Registers** - General Purpose

* Fast storage options in processor - data transferred from memory to registers for faster processing. 
* Limited in number - usually only 16 to 32. Compiler associates variables in program with registers.
* Registers have no data type - Machine/Assembly instructions assume data stored in register is correct type. 

32 Registers in MIPS Assembly Language:

| Name     |  Reg Number | Usage |
| ------------- |:-------------:| -----:|
| `$zero`     | 0 | Constant hardcoded 0 value |
| `$v0-$v1`    | 2-3    |   Values for results & expression evaluation |
| `$a0-a3` | 4-7      |    Arguments eg for syscall |
| `$t0-$t7` | 8-15    |   Temporaries |
| `$s0-$s7` | 16-23   |  Program Variables | 
| `$t8-$t9` | 24-25   |  More temporaries |
| `$gp` | 28  |  Global Pointer |
| `$sp` | 29  |  Stack Pointer |
| `$fp` | 30  |  Frame Pointer |
| `$ra` | 31  |  Return Address |

Can be referred by a number eg `$16` is `$s0` or name eg `$a0`. `$at` register 1 is reserved for assembler. Registers 26-27 reserved for OS. 

**MIPS Instructions**

Each instruction executes a single command. Each line has at most 1 instruction. # used for comments. General syntax:

`add $s0, $s1, $s2` implies `$s0` stores result of `$s1 + $s2`

Equivalent C code: `a = b + c` where `a = s0, b = s1, c = s2`. Variable Mapping - registers to value. 

A single MIPS instruction can handle at most two source operands. 

Logical Operations: View 32 bit number not as whole but 32 different bits. 

| Operation    |  MIPS version | MIPS Immediate Version (if available) | Use case |
| ------------- |:-------------:| :-----:|  :-----:|
| Addition     | 0 | Constant hardcoded 0 value |
| Subtraction   | 2-3    |   Values for results & expression evaluation |
| Shift Left Logical | 4-7      |    Arguments eg for syscall |
| Shift Right Logical | 8-15    |   Temporaries |
| AND bitwise | 16-23   |  Program Variables | 
| OR bitewise | 24-25   |  More temporaries |
| OR bitewise | 28  |  Global Pointer |
| NOR bitewise | 29  |  Stack Pointer |
| XOR bitewise | 30  |  Frame Pointer |













































