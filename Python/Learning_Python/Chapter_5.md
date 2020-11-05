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
