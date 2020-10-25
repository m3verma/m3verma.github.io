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

To know what a function does, pass them to **help** function. help is one of a handful of interfaces to a system of code that ships with Python known as PyDoc—a tool for extracting documentation from objects.

```python
>>> help(S.replace)
```

### Other Ways to Code Strings

Python also provides a variety of ways for us to code strings. Python allows strings to be enclosed in single or double quote characters—they mean the same thing but allow the other type of quote to be embedded with an escape. It also allows multiline string literals enclosed in triple quotes (single or double)—when this form is used, all the lines are concatenated together, and end-of-line characters are added where line breaks appear.

```python
>>> S = 'A\nB\tC'            # \n is end-of-line, \t is tab
>>> len(S)                   # Each stands for just one character
```
> 5

```python
>>> ord('\n')                # \n is a byte with the binary value 10 in ASCII
```
> 10

```python
>>> S = 'A\0B\0C'            # \0, a binary zero byte, does not terminate string
>>> len(S)
```
> 5

```python
>>> msg = """
aaaaaaaaaaaaa
bbb'''bbbbbbbbbb""bbbbbbb'bbbb
cccccccccccccc
"""
>>> msg
```
> '\naaaaaaaaaaaaa\nbbb\'\'\'bbbbbbbbbb""bbbbbbb\'bbbb\ncccccccccccccc\n'

### Pattern Matching

Text pattern matching is an advanced tool which can be done by importing a module called **re**. This module has analogous calls for searching, splitting, and replacement.

```python
>>> import re
>>> match = re.match('Hello[ \t]*(.*)world', 'Hello    Python world')
>>> match.group(1)
```
> 'Python'

This example searches for a substring that begins with the word “Hello,” followed by zero or more tabs or spaces, followed by arbitrary characters to be saved as a matched group, terminated by the word “world.” If such a substring is found, portions of the substring matched by parts of the pattern enclosed in parentheses are available as groups.

### Lists

Lists are positionally ordered collections of arbitrarily typed objects, and they have no fixed size. They are also mutable—unlike strings, lists can be modified in place by assignment to offsets as well as a variety of list method calls. Accordingly, they provide a very flexible tool for representing arbitrary collections—lists of files in a folder, employees in a company, emails in your inbox, and so on. Because they are sequences, lists support all the sequence operations we discussed for strings; the only difference is that the results are usually lists instead of strings.

```python
>>> L = [123, 'spam', 1.23]            # A list of three different-type objects
>>> len(L)                             # Number of items in the list
```
> 3

```python
>>> L[0]                               # Indexing by position
```
> 123

```python
>>> L + [4, 5, 6]                      # Concat/repeat make new lists too
```
> 123, 'spam', 1.23, 4, 5, 6

```python
>>> L                                  # We're not changing the original list
```
> 123, 'spam', 1.23

Python’s lists may be reminiscent of arrays in other languages, but they tend to be more powerful. For one thing, they have no fixed type constraint—the list we just looked at, for example, contains three objects of completely different types (an integer, a string, and a floating-point number). Further, lists have no fixed size. That is, they can grow and shrink on demand. Because lists are mutable, most list methods also change the list object in place, instead of creating a new one.

```python
>>> M = ['bb', 'aa', 'cc']
>>> M.sort()
>>> M
```
> 'aa', 'bb', 'cc'

Python still doesn’t allow us to reference items that are not present. Indexing off the end of a list is always a mistake, but so is assigning off the end. Python includes a more advanced operation known as a list comprehension expression, which turns out to be a powerful way to process structures like our matrix. Suppose, for instance, that we need to extract the second column of our sample matrix. It’s easy to grab rows by simple indexing because the matrix is stored by rows, but it’s almost as easy to get a column with a list comprehension

```python
>>> col2 = [row[1] for row in M]             # Collect the items in column 2
>>> col2
```
> 2, 5, 8

### Dictionaries

Python dictionaries are something completely different—they are not sequences at all, but are instead known as mappings. Mappings are also collections of other objects, but they store objects by key instead of by relative position. In fact, mappings don’t maintain any reliable left-to-right order; they simply map keys to associated values. Dictionaries, the only mapping type in Python’s core objects set, are also mutable.

```python
>>> D = {'food': 'Spam', 'quantity': 4, 'color': 'pink'}
>>> D['food']              # Fetch value of key 'food'
```
> 'Spam'

```python
>>> D['quantity'] += 1     # Add 1 to 'quantity' value
>>> D
```
> {'color': 'pink', 'food': 'Spam', 'quantity': 5}

The following code, for example, starts with an empty dictionary and fills it out one key at a time. Unlike out-of-bounds assignments in lists, which are forbidden, assignments to new dictionary keys create those keys

```python
>>> D = {}
>>> D['name'] = 'Bob'      # Create keys by assignment
>>> D['job']  = 'dev'
>>> D['age']  = 40
>>> D
```
> {'age': 40, 'job': 'dev', 'name': 'Bob'}

Both the following make the same dictionary as the prior example and its equivalent {} literal form, though the first tends to make for less typing.

```python
>>> bob1 = dict(name='Bob', job='dev', age=40)                      # Keywords
>>> bob1
```
> {'age': 40, 'name': 'Bob', 'job': 'dev'}

```python
>>> bob2 = dict(zip(['name', 'job', 'age'], ['Bob', 'dev', 40]))    # Zipping
>>> bob2
```
> {'job': 'dev', 'name': 'Bob', 'age': 40}

Suppose, that the information is more complex. Perhaps we need to record a first name and a last name, along with multiple job titles. This leads to another application of Python’s object nesting in action. Nesting allows us to build up complex information structures directly and easily

```python
>>> rec = {'name': {'first': 'Bob', 'last': 'Smith'},
           'jobs': ['dev', 'mgr'],
           'age':  40.5}
>>> rec['name']                         # 'name' is a nested dictionary
```
> {'last': 'Smith', 'first': 'Bob'}

```python
>>> rec['name']['last']                 # Index the nested dictionary
```
> 'Smith'

In Python, when we lose the last reference to the object—by assigning its variable to something else, for example—all of the memory space occupied by that object’s structure is automatically cleaned up for us. Python has a feature known as garbage collection that cleans up unused memory as your program runs and frees you from having to manage such details in your code.

```python
>>> rec = 0                             # Now the object's space is reclaimed
```

Although we can assign to a new key to expand a dictionary, fetching a nonexistent key is still a mistake.

```python
>>> D = {'a': 1, 'b': 2, 'c': 3}
>>> D['f']                           # Referencing a nonexistent key is an error
```
> ...error text omitted... KeyError: 'f'

To identify a missing key we can use 'in' keyword.

```python
>>> 'f' in D
```
> False

As mentioned earlier, because dictionaries are not sequences, they don’t maintain any dependable left-to-right order. If we make a dictionary and print it back, its keys may come back in a different order than that in which we typed them, and may vary per Python version and other variables. To make dictionary in order :

```python
>>> Ks = list(D.keys())                # Unordered keys list
>>> Ks                                 # A list in 2.X, "view" in 3.X: use list()
```
> 'a', 'c', 'b'

```python
>>> Ks.sort()                          # Sorted keys list
>>> Ks
```
> 'a', 'b', 'c'

```python
>>> for key in Ks:                     # Iterate though sorted keys
        print(key, '=>', D[key])       # <== press Enter twice here (3.X print)
```
> a => 1
> b => 2
> c => 3

```python
>>> for key in sorted(D):
        print(key, '=>', D[key])        # Easier way to do it
```
> a => 1
> b => 2
> c => 3



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
