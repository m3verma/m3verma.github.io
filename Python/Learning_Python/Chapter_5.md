---
layout: default
---

# Numeric Types

### Numeric Type Basics

In Python, numbers are not really a single object type, but a category of similar types. Python supports the usual numeric types (integers and floating points), as well as literals for creating numbers and expressions for processing them. In addition, Python provides more advanced numeric programming support and objects for more advanced work. A complete inventory of Python’s numeric toolbox includes:
1. Integer and floating-point objects
2. Complex number objects
3. Decimal: fixed-precision objects
4. Fraction: rational number objects
5. Sets: collections with numeric operations
6. Booleans: true and false
7. Built-in functions and modules: round, math, random, etc.
8. Expressions; unlimited integer precision; bitwise operations; hex, octal, and binary formats
9. Third-party extensions: vectors, libraries, visualization, plotting, etc.

### Numeric Literals

Python provides integers, which are positive and negative whole numbers, and floating-point numbers, which are numbers with a fractional part. Python also allows us to write integers using hexadecimal, octal, and binary literals; offers a complex number type; and allows integers to have unlimited precision—they can grow to have as many digits as your memory space allows.

| Literal        | Interpretation          |
|:-------------|:------------------|
| 1234, −24, 0, 99999999999999           | Integers (unlimited size) |
| 1.23, 1., 3.14e-10, 4E210, 4.0e+210 | Floating-point numbers  |
| 0o177, 0x9ff, 0b101010           | Octal, hex, and binary literals in 3.X  |
| 3+4j, 3.0+4.0j, 3J           | Complex number literals |
| set('spam'), {1, 2, 3, 4}           | Sets: 2.X and 3.X construction forms |
| Decimal('1.0'), Fraction(1, 3) | Decimal and fraction extension types  |
| bool(X), True, False           | set('abc'), {'a', 'b', 'c'}      |
| Other core types           | Boolean type and constants |

1. **Integer and Floating-point literals** - Integers are written as strings of decimal digits. Floating-point numbers have a decimal point and/or an optional signed exponent introduced by an e or E and followed by an optional sign.
2. **Hexadecimal, octal, and binary literals** - Integers may be coded in decimal (base 10), hexadecimal (base 16), octal (base 8), or binary (base 2), the last three of which are common in some programming domains. Hexadecimals start with a leading 0x or 0X, followed by a string of hexadecimal digits (0–9 and A–F). Octal literals start with a leading 0o or 0O, followed by a string of digits (0–7). Binary literals, begin with a leading 0b or 0B, followed by binary digits (0–1).
3. **Complex numbers** - Python complex literals are written as realpart+imaginarypart, where the imaginarypart is terminated with a j or J. The realpart is technically optional, so the imaginarypart may appear on its own.

### Built-in Numeric Tools

Python provides a set of tools for processing number objects:

1. **Expression operators**
2. **Built-in mathematical functions**
3. **Utility modules**

### Python Expression Operators

Perhaps the most fundamental tool that processes numbers is the **expression**: a combination of numbers (or other objects) and operators that computes a value when executed by Python. All the operator expressions available in Python are :

| Operators        | Description          |
|:-------------|:------------------|
| yield x           | Generator function send protocol |
| lambda args: expression | Anonymous function generation  |
| x if y else z           | Ternary selection (x is evaluated only if y is true)  |
| x or y           | Logical OR (y is evaluated only if x is false) |
| x and y           | Logical AND (y is evaluated only if x is true) |
| not x | Logical negation  |
| x in y, x not in y           | Membership (iterables, sets)  |
| x is y, x is not y         | Object identity tests |
| x < y, x <= y, x > y, x >= y         | Magnitude comparison, set subset and superset; |
| x == y,x != y           | Value equality operators   |
| `x | y`         | Bitwise OR, set union |
| x ^ y         | Bitwise XOR, set symmetric difference |
| x & y         | Bitwise AND, set intersection |
| x << y, x >> y         | Shift x left or right by y bits |
| x + y         | Addition, concatenation; |
| x - y         | Subtraction, set difference |
| x * y         | Multiplication, repetition; |
| x % y         | Remainder, format; |
| x / y,x // y         | Division: true and floor |
| -x, +x         | Negation, identity |
| x ** y        | Power (exponentiation) |
| `x[i]`         | Indexing (sequence, mapping, others) |
| `x[i:j:k]`         | Slicing |
| x(...)        | Call (function, method, class, other callable) |
| x.attr        | Attribute reference |
| (...)        | Tuple, expression, generator expression |
| `[...]`        | List, list comprehension |
| {...}        | Dictionary, set, set and dictionary comprehensions |

### Operator Precedence

When you write an expression with more than one operator, Python groups its parts according to what are called precedence rules, and this grouping determines the order in which the expression’s parts are computed. The above table is ordered by operator precedence :
1. Operators lower in the table have higher precedence, and so bind more tightly in mixed expressions.
2. Operators in the same row in Table generally group from left to right when combined (except for exponentiation, which groups right to left, and comparisons, which chain left to right).

For example, if you write X + Y * Z, Python evaluates the multiplication first (Y * Z), then adds that result to X because * has higher precedence (is lower in the table) than +.

When you enclose subexpressions in parentheses, you override Python’s precedence rules; Python always evaluates expressions in parentheses first before using their results in the enclosing expressions. For instance, instead of coding X + Y * Z, you could write one of the following to force Python to evaluate the expression in the desired order:

```
(X + Y) * Z
X + (Y * Z)
```

### Mixed types are converted up

Besides mixing operators in expressions, you can also mix numeric types. For instance, you can add an integer to a floating-point number:

> 40 + 3.14

But this leads to another question: what type is the result—integer or floating point? The answer is simple, especially if you’ve used almost any other language before: in mixed-type numeric expressions, Python first converts operands up to the type of the most complicated operand, and then performs the math on same-type operands. Also, keep in mind that all these mixed-type conversions apply only when mixing numeric types (e.g., an integer and a floating point) in an expression, including those using numeric and comparison operators. In general, Python does not convert across any other type boundaries automatically. Adding a string to an integer, for example, results in an error, unless you manually convert one or the other.

### Variables and Basic Expressions

In the following interaction, we first assign two variables (a and b) to integers so we can use them later in a larger expression. Variables are simply names—created by you or Python—that are used to keep track of information in your program. In Python:

1. Variables are created when they are first assigned values.
2. Variables are replaced with their values when used in expressions.
3. Variables must be assigned before they can be used in expressions.
4. Variables refer to objects and are never declared ahead of time.

```python
>>> a = 3         #This is a Comment
>>> b = 4
```

In Python code, text after a # mark and continuing to the end of the line is considered to be a comment and is ignored by Python. Comments are a way to write human-readable documentation for your code, and an important part of programming.

```python
>>> a + 1, a −1           # Addition (3 + 1), subtraction (3 − 1)
```
> (4, 2)

```python
>>> b * 3, b / 2           # Multiplication (4 * 3), division (4 / 2)
```
> (12, 2.0)

```python
>>> a % 2, b ** 2          # Modulus (remainder), power (4 ** 2)
```
> (1, 16)

```python
>>> 2 + 4.0, 2.0 ** b      # Mixed-type conversions
```
> (6.0, 16.0)

Technically, the results being echoed back here are tuples of two values because the lines typed at the prompt contain two expressions separated by commas; that’s why the results are displayed in parentheses. You don’t need to predeclare variables in Python, but they must have been assigned at least once before you can use them.

### Numeric Display Formats

There are more ways to display the bits of a number inside your computer than using print and automatic echoes.

```python
>>> num = 1 / 3.0
>>> num                      # Auto-echoes
```
> 0.3333333333333333

```python
>>> print(num)               # Print explicitly
```
> 0.3333333333333333

```python
>>> '%e' % num               # String formatting expression
```
> '3.333333e-01'

```python
>>> '%4.2f' % num            # Alternative floating-point format
```
> '0.33'

```python
>>> '{0:4.2f}'.format(num)   # String formatting method: Python 2.6, 3.0, and later
```
> '0.33'

### Comparisons : Normal and Chained

So far, we’ve been dealing with standard numeric operations (addition and multiplication), but numbers, like all Python objects, can also be compared. Normal comparisons work for numbers exactly as you’d expect—they compare the relative magnitudes of their operands and return a Boolean result.

```python
>>> 1 < 2                  # Less than
```
> True

```python
>>> 2.0 >= 1               # Greater than or equal: mixed-type 1 converted to 1.0
```
> True

```python
>>> 2.0 == 2.0             # Equal value
```
> True

```python
>>> 2.0 != 2.0             # Not equal value
```
> False

Python also allows us to chain multiple comparisons together to perform range tests. Chained comparisons are a sort of shorthand for larger Boolean expressions.

```python
>>> X = 2
>>> Y = 4
>>> Z = 6
>>> X < Y < Z              # Chained comparisons: range tests
```
> True

```python
>>> 1 < 2 < 3.0 < 4
```
> True

Numeric comparisons are based on magnitudes, which are generally simple—though floating-point numbers may not always work as you’d expect, and may require conversions or other massaging to be compared meaningfully:

```python
>>> 1.1 + 2.2 == 3.3             # Shouldn't this be True?...
```
> False

```python
>>> 1.1 + 2.2                    # Close to 3.3, but not exactly: limited precision
```
> 3.3000000000000003

```python
>>> int(1.1 + 2.2) == int(3.3)   # OK if convert: see also round, floor, trunc ahead   
```
> True

### Division : Classic, Floor and True

1. **X / Y** - Classic and true division. In Python 2.X, this operator performs classic division, truncating results for integers, and keeping remainders (i.e., fractional parts) for floating-point numbers. In Python 3.X, it performs true division, always keeping remainders in floating-point results, regardless of types.
2. **X // Y** - Floor division. Added in Python 2.2 and available in both Python 2.X and 3.X, this operator always truncates fractional remainders down to their floor, regardless of types. Its result type depends on the types of its operands.

```python
>>> 10 / 4            # Differs in 3.X: keeps remainder
```
> 2.5

```python
>>> 10 / 4.0          # Same in 3.X: keeps remainder
```
> 2.5

```python
>>> 10 // 4           # Same in 3.X: truncates remainder
```
> 2

```python
>>> 10 // 4.0           # Same in 3.X: truncates to floor
```
> 2.0

The `//` operator is informally called truncating division, but it’s more accurate to refer to it as floor division—it truncates the result down to its floor, which means the closest whole number below the true result.

```python
>>> 5 / 2, 5 / −2
```
> (2.5, −2.5)

```python
>>> 5 // 2, 5 // −2           # Truncates to floor: rounds to first lower integer
```
> (2, −3)                       # 2.5 becomes 2, −2.5 becomes −3

```python
>>> 5 / 2.0, 5 / −2.0
```
> (2.5, −2.5)

```python
>>> 5 // 2.0, 5 // −2.0       # Ditto for floats, though result is float too
```
> (2.0, −3.0)

### Integer Precision

Because Python must do extra work to support their extended precision, integer math is usually substantially slower than normal when numbers grow large. However, if you need the precision, the fact that it’s built in for you to use will likely outweigh its performance penalty.

```python
>>> 2 ** 200
```
> 1606938044258990275541962092341162602522202993782792835301376

### Complex Numbers

Complex numbers are represented as two floating-point numbers—the real and imaginary parts—and you code them by adding a j or J suffix to the imaginary part. We can also write complex numbers with a nonzero real part by adding the two parts with a +. For example, the complex number with a real part of 2 and an imaginary part of −3 is written 2 + −3j. Here are some examples of complex math at work:

```python
>>> 1j * 1J
```
> (-1+0j)

```python
>>> 2 + 1j * 3
```
> (2+3j)

```python
>>> (2 + 1j) * 3
```
> (6+3j)

### Hex, Octal, Binary : Literals and Conversions

Python integers can be coded in hexadecimal, octal, and binary notation, in addition to the normal base-10 decimal coding we’ve been using so far. The first three of these may at first seem foreign to 10-fingered beings, but some programmers find them convenient alternatives for specifying values, especially when their mapping to bytes and bits is important.

```python
>>> 0o1, 0o20, 0o377           # Octal literals: base 8, digits 0-7 (3.X, 2.6+)
```
> (1, 16, 255)

```python
>>> 0x01, 0x10, 0xFF           # Hex literals: base 16, digits 0-9/A-F (3.X, 2.X)
```
> (1, 16, 255)

```python
>>> 0b1, 0b10000, 0b11111111   # Binary literals: base 2, digits 0-1 (3.X, 2.6+)
```
> (1, 16, 255)

Python prints integer values in decimal (base 10) by default but provides built-in functions that allow you to convert integers to other bases’ digit strings, in Python-literal form—useful when programs or users expect to see values in a given base :

```python
>>> oct(64), hex(64), bin(64)               # Numbers=>digit strings
```
> ('0o100', '0x40', '0b1000000')

To go the other way, the built-in int function converts a string of digits to an integer, and an optional second argument lets you specify the numeric base—useful for numbers read from files as strings instead of coded in scripts :

```python
>>> int('64'), int('100', 8), int('40', 16), int('1000000', 2)
```
> (64, 64, 64, 64)

The eval function, treats strings as though they were Python code. Therefore, it has a similar effect, but usually runs more slowly—it actually compiles and runs the string as a piece of a program, and it assumes the string being run comes from a trusted source—a clever user might be able to submit a string that deletes files on your machine, so be careful with this call :

```python
>>> eval('64'), eval('0o100'), eval('0x40'), eval('0b1000000')
```
> (64, 64, 64, 64)

### Bitwise Operations

Python supports most of the numeric expressions available in the C language. This includes operators that treat integers as strings of binary bits, and can come in handy if your Python code must deal with things like network packets, serial ports, or packed binary data produced by a C program.

```python
>>> x = 1               # 1 decimal is 0001 in bits
>>> x << 2              # Shift left 2 bits: 0100
```
> 4

```python
>>> x | 2               # Bitwise OR (either bit=1): 0011
```
> 3

```python
>>> x & 1               # Bitwise AND (both bits=1): 0001
```
> 1

Also in this department, Python 3.1 and 2.7 introduced a new integer bit_length method, which allows you to query the number of bits required to represent a number’s value in binary.

```python
>>> X = 99
>>> bin(X), X.bit_length(), len(bin(X)) - 2
```
> ('0b1100011', 7, 7)

### Other Built-in Numeric Tools

In addition to its core object types, Python also provides both built-in functions and standard library modules for numeric processing. The pow and abs built-in functions, for instance, compute powers and absolute values, respectively. Here are some examples of the built-in math module.

```python
>>> import math
>>> math.pi, math.e                               # Common constants
```
> (3.141592653589793, 2.718281828459045)

```python
>>> math.sin(2 * math.pi / 180)                   # Sine, tangent, cosine
```
> 0.03489949670250097

```python
>>> math.sqrt(144), math.sqrt(2)                  # Square root
```
> (12.0, 1.4142135623730951)

```python
>>> pow(2, 4), 2 ** 4, 2.0 ** 4.0                 # Exponentiation (power)
```
> (16, 16, 16.0)

```python
>>> abs(-42.0), sum((1, 2, 3, 4))                 # Absolute value, summation
```
> (42.0, 10)

```python
>>> min(3, 1, 2, 4), max(3, 1, 2, 4)              # Minimum, maximum
```
> (1, 4)

Interestingly, there are three ways to compute square roots in Python: using a module function, an expression, or a built-in function.

```python
>>> import math
>>> math.sqrt(144)              # Module
```
> 12.0

```python
>>> 144 ** .5                   # Expression
```
> 12.0

```python
>>> pow(144, .5)                # Built-in
```
> 12.0

The standard library random module must be imported as well. This module provides an array of tools, for tasks such as picking a random floating-point number between 0 and 1, and selecting a random integer between two numbers. This module can also choose an item at random from a sequence, and shuffle a list of items randomly.

```python
>>> import random
>>> random.random()
```
> 0.5566014960423105

```python
>>> random.random()              # Random floats, integers, choices, shuffles
```
> 0.051308506597373515

```python
>>> random.randint(1, 10)
```
> 5

```python
>>> random.choice(['Life of Brian', 'Holy Grail', 'Meaning of Life'])
```
> 'Holy Grail'

### Decimal Type

Python 2.4 introduced a new core numeric type: the decimal object, formally known as Decimal. Syntactically, you create decimals by calling a function within an imported module, rather than running a literal expression. Functionally, decimals are like floating-point numbers, but they have a fixed number of decimal points. Hence, decimals are fixed-precision floating-point values.

```python
>>> from decimal import Decimal
>>> Decimal('0.1') + Decimal('0.1') + Decimal('0.1') - Decimal('0.3')
```
> Decimal('0.0')

As shown here, we can make decimal objects by calling the Decimal constructor function in the decimal module and passing in strings that have the desired number of decimal digits for the resulting object (using the str function to convert floating-point values to strings if needed). When decimals of different precision are mixed in expressions, Python converts up to the largest number of decimal digits automatically:

```python
>>> Decimal('0.1') + Decimal('0.10') + Decimal('0.10') - Decimal('0.30')
```
> Decimal('0.00')

Other tools in the decimal module can be used to set the precision of all decimal numbers, arrange error handling, and more. For instance, a context object in this module allows for specifying precision (number of decimal digits) and rounding modes (down, ceiling, etc.). The precision is applied globally for all decimals created in the calling thread:

```python
>>> import decimal
>>> decimal.Decimal(1) / decimal.Decimal(7)                     # Default: 28 digits
```
> Decimal('0.1428571428571428571428571429')

```python
>>> decimal.getcontext().prec = 4                               # Fixed precision
>>> decimal.Decimal(1) / decimal.Decimal(7)
```
> Decimal('0.1429')

It’s also possible to reset precision temporarily by using the with context manager statement. The precision is reset to its original value on statement exit.

```python
>>> with decimal.localcontext() as ctx:
...     ctx.prec = 2
...     decimal.Decimal('1.00') / decimal.Decimal('3.00')
...
```
> Decimal('0.33')

### Fraction Type

Python 2.6 and 3.0 debuted a new numeric type, Fraction, which implements a rational number object. It essentially keeps both a numerator and a denominator explicitly, so as to avoid some of the inaccuracies and limitations of floating-point math.

```python
>>> from fractions import Fraction
>>> x = Fraction(1, 3)                    # Numerator, denominator
>>> y = Fraction(4, 6)                    # Simplified to 2, 3 by gcd
>>> x
```
> Fraction(1, 3)

```python
>>> x + y
```
> Fraction(1, 1)

```python
>>> x - y
```
> Fraction(-1, 3)

Fraction objects can also be created from floating-point number strings, much like decimals:

```python
>>> Fraction('.25')
```
> Fraction(1, 4)

In fact, fractions both retain accuracy and automatically simplify results. Continuing the preceding interaction:

```python
>>> Fraction(6, 12)
```
> Fraction(1, 2)

### Sets

Besides decimals, Python 2.4 also introduced a new collection type, the set—an unordered collection of unique and immutable objects that supports operations corresponding to mathematical set theory. By definition, an item appears only once in a set, no matter how many times it is added. However, because sets are unordered and do not map keys to values, they are neither sequence nor mapping types; they are a type category unto themselves.

```python
>>> set([1, 2, 3, 4])            # Built-in: same as in 2.6
```
> {1, 2, 3, 4}

```python
>>> set('spam')                  # Add all items in an iterable
```
> {'s', 'a', 'p', 'm'}

```python
>>> S1 = {1, 2, 3, 4}
>>> S1 & {1, 3}                  # Intersection
```
> {1, 3}

```python
>>> {1, 5, 3, 6} | S1            # Union
```
> {1, 2, 3, 4, 5, 6}

```python
>>> S1 > {1, 3}                  # Superset
```
> True

Sets are powerful and flexible objects, but they do have one constraint in both 3.X and 2.X that you should keep in mind—largely because of their implementation, sets can only contain immutable (a.k.a. “hashable”) object types. Set comprehensions run a loop and collect the result of an expression on each iteration; a loop variable gives access to the current iteration value for use in the collection expression. The result is a new set you create by running the code, with all the normal set behavior.

```python
>>> {x ** 2 for x in [1, 2, 3, 4]}         # 3.X/2.7 set comprehension
```
> {16, 1, 4, 9}

Set operations have a variety of common uses, some more practical than mathematical. For example, because items are stored only once in a set, sets can be used to filter duplicates out of other collections, though items may be reordered in the process because sets are unordered in general. Simply convert the collection to a set, and then convert it back again.


```python
>>> L = [1, 2, 1, 3, 2, 4, 5]
>>> set(L)
```
> {1, 2, 3, 4, 5}

```python
>>> set([1, 3, 5, 7]) - set([1, 2, 4, 5, 6])          # Find list differences
```
> {3, 7}

You can also use sets to perform order-neutral equality tests by converting to a set before the test, because order doesn’t matter in a set. More formally, two sets are equal if and only if every element of each set is contained in the other—that is, each is a subset of the other, regardless of order. Finally, sets are also convenient when you’re dealing with large data sets (database query results, for example)—the intersection of two sets contains objects common to both categories, and the union contains all items in either set.

```python
>>> engineers = {'bob', 'sue', 'ann', 'vic'}
>>> managers  = {'tom', 'sue'}
>>> 'bob' in engineers                   # Is bob an engineer?
```
> True

```python
>>> engineers & managers                 # Who is both engineer and manager?
```
> {'sue'}


* * *

# Test Your Knowledge

### Q1 - Name four of Python’s core data types.

```
Numbers, strings, lists, dictionaries, tuples, files, and sets are generally considered to be the 
core object (data) types. Types, None, and Booleans are sometimes classified this way as well. 
There are multiple number types (integer, floating point, complex, fraction, and decimal) and 
multiple string types (simple strings and Unicode strings in Python 2.X, and text strings and byte 
strings in Python 3.X).
```
