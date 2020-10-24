---
layout: default
---

# Introducing Python Object Types

### The Python Conceptual Hierarchy

Python programs can be decomposed into modules, statements, expressions, and objects, as follows :

1. Programs are composed of modules.
2. Modules contain statements.
3. Statements contain expressions.
4. Expressions create and process objects.

### Why Use Built-in Types?

Python provides powerful object types as an intrinsic part of the language, there’s usually no need to code object implementations before you start solving problems. In fact, unless you have a need for special processing that built-in types don’t provide, you’re almost always better off using a built-in object instead of implementing your own. Here are some reasons why:

1. Built-in objects make programs easy to write.
2. Built-in objects are components of extensions.
3. Built-in objects are often more efficient than custom data structures.
4. Built-in objects are a standard part of the language.

### Python's Core Data Types

Once you create an object, you bind its operation set for all time you can perform only string operations on a string and list operations on a list. In formal terms, this means that Python is dynamically typed, a model that keeps track of types for you automatically instead of requiring declaration code, but it is also strongly typed, a constraint that means you can perform on an object only operations that are valid for its type.

| Object type        | Example          |
|:-------------|:------------------|
| Numbers           | 1234, 3.1415, 3+4j, Decimal() |
| Strings | 'spam', "Bob's", "b'a\x01c"   |
| Lists           | `[1, [2, 'three']]`, list(range(10))      |
| Dictionaries           | {'food': 'spam', 4, 'U'}, dict(hours=10) |
| Tuples           | (1, 'spam', 4, 'U'), tuple('spam') |
| Files | open('eggs.txt')   |
| Sets           | set('abc'), {'a', 'b', 'c'}      |
| Other core types           | Booleans, types, None |
| Program unit types           | Functions, modules, classes |
| Implementation-related types           | Compiled code, stack tracebacks |

### Numbers

Python's core objects includes : integers that have no fractional part, floating-point numbers that do, and more exotic types—complex numbers with imaginary parts, decimals with fixed precision, rationals with numerator and denominator, and full-featured sets. Built-in numbers are enough to represent most numeric quantities—from your age to your bank balance—but more types are available as third-party add-ons.

```python
>>> 123 + 222                    # Integer addition
```
> 345

```python
>>> 1.5 * 4                      # Floating-point multiplication
```
> 6.0

```python
>>> 2 ** 100                     # 2 to the power 100, again
```
> 1267650600228229401496703205376

```python
>>> len(str(2 ** 1000000))       # How many digits in a really BIG number?
```
> 301030

```python
>>> 3.1415 * 2                   # repr: as code (Pythons >= 2.7 and 3.1)
```
> 6.283

Besides expressions, there are a handful of useful modules that ship with Python. The **math** module contains more advanced numeric tools as functions

```python
>>> import math
>>> math.pi
```
> 3.141592653589793

```python
>>> math.sqrt(85)
```
> 9.219544457292887

```python
>>> import random       #Perform random number generation
>>> random.random()
```
> 0.7082048489415967

```python
>>> random.choice([1, 2, 3, 4])
```
> 1

### Strings

Strings are used to record both textual information as well as arbitrary collection of bytes (image file's contents). These are also called **sequence** - a positionally ordered collection of other objects.

```python
>>> S = 'Spam'           # Make a 4-character string, and assign it to a name
>>> len(S)               # Length
```
> 4

```python
>>> S[0]                 # The first item in S, indexing by zero-based position
```
> 'S'

```python
>>> S[1]                 # The second item from the left
```
> 'p'

Indexes are coded as offsets from the front, and so start from 0. Here positive indexes count from left and negative indexes count from the right.

```python
>>> S[-1]                 # The last item from the end in S
```
> 'm'

```python
>>> S[-2]                 # The second-to-last item from the end in S
```
> 'a'

Slicing - A way to extract an entire section (slice) in a single step. Their general form, `X[I:J]`, means “give me everything in X from offset I up to but not including offset J.”. In a slice, the left bound defaults to zero, and the right bound defaults to the length of the sequence being sliced.

```python
>>> S[1:3]                 # Slice of S from offsets 1 through 2 (not 3)
```
> 'pa'

```python
>>> S[1:]                 # Everything past the first (1:len(S))
```
> 'pam'

```python
>>> S                     # S itself hasn't changed
```
> 'Spam'

```python
>>> S[0:3]                # Everything but the last
```
> 'Spa'

```python
>>> S[:3]                 # Same as S[0:3]
```
> 'Spa'

```python
>>> S[:-1]                # Everything but the last again, but simpler (0:-1)
```
> 'Spa'

```python
>>> S[:]                  # All of S as a top-level copy (0:len(S))
```
> 'Spam'

### Immutability

Every string operation is defined to produce a new string as its result, because strings are immutable in Python. They cannot be changed in place after they are created. In terms of core types, numbers, strings and tuples are immutable; lists, dictionaries and sets are not.

```python
>>> S[0] = 'z'             # Immutable objects cannot be changed
```
> TypeError: 'str' object does not support item assignment

```python
>>> S = 'z' + S[1:]        # But we can run expressions to make new objects
>>> S
```
> 'zpam'

### Type-Specific Methods

In addition to generic sequence operations, though, strings also have operations all their own, available as methods—functions that are attached to and act upon a specific object, which are triggered with a call expression.

```python
>>> S = 'Spam'
>>> S.find('pa')                 # Find the offset of a substring in S
```
> 1

```python
>>> S.replace('pa', 'XYZ')       # Replace occurrences of a string in S with another
```
> 'SXYZm'

Despite the names of these string methods, we are not changing the original strings here, but creating new strings as results.

```python
>>> S = 'spam'
>>> S.upper()                    # Upper- and lowercase conversions
```
> 'SPAM'

```python
>>> line = 'aaa,bbb,ccccc,dd\n'
>>> line.rstrip()                # Remove whitespace characters on the right side
```
> 'aaa,bbb,ccccc,dd'

Strings also support an advanced substitution operation known as formatting, available as both an expression (the original) and a string method call (new as of 2.6 and 3.0)

```python
>>> '%s, eggs, and %s' % ('spam', 'SPAM!')          # Formatting expression (all)
```
> 'spam, eggs, and SPAM!'

```python
>>> '{0}, eggs, and {1}'.format('spam', 'SPAM!')    # Formatting method (2.6+, 3.0+)
```
> 'spam, eggs, and SPAM!'

### Getting Help

Function **dir** lists variables assigned in the caller’s scope when called with no argument; more usefully, it returns a list of all the attributes available for any object passed to it. Assuming S is a string :

```python
>>> dir(S)
```
> '__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill'

* * *

# Test Your Knowledge

### Q1 - How can you start an interactive interpreter session?

```
You can start an interactive session on Windows 7 and earlier by clicking your Start button, picking 
the All Programs option, clicking the Python entry, and selecting the “Python (command line)” menu 
option. You can also achieve the same effect on Windows and other platforms by typing python as a 
system command line in your system’s console window (a Command Prompt window on Windows). Another 
alternative is to launch IDLE, as its main Python shell window is an interactive session. Depending 
on your platform and Python, if you have not set your system’s PATH variable to find Python, you may 
need to cd to where Python is installed, or type its full directory path instead of just python 
(e.g., C:\Python33\python on Windows, unless you’re using the 3.3 launcher)
```
