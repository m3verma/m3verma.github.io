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

1. Integer and Floating-point literals - Integers are written as strings of decimal digits. Floating-point numbers have a decimal point and/or an optional signed exponent introduced by an e or E and followed by an optional sign.
2. Hexadecimal, octal, and binary literals - Integers may be coded in decimal (base 10), hexadecimal (base 16), octal (base 8), or binary (base 2), the last three of which are common in some programming domains. Hexadecimals start with a leading 0x or 0X, followed by a string of hexadecimal digits (0–9 and A–F). Octal literals start with a leading 0o or 0O, followed by a string of digits (0–7). Binary literals, begin with a leading 0b or 0B, followed by binary digits (0–1).
3. Complex numbers - Python complex literals are written as realpart+imaginarypart, where the imaginarypart is terminated with a j or J. The realpart is technically optional, so the imaginarypart may appear on its own.

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
