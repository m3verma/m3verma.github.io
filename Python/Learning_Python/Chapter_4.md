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

### Iteration and Optimization

An object is **Iterable** if it is either a physically stored sequence in memory, or an object that generates one item at a time in the context of an iteration operation—a sort of “virtual” sequence. More formally, both types of objects are considered iterable because they support the iteration protocol—they respond to the iter call with an object that advances in response to next calls and raises an exception when finished producing values. Every Python tool that scans an object from left to right uses the iteration protocol.

```python
>>> squares = [x ** 2 for x in [1, 2, 3, 4, 5]]
>>> squares
```
> 1, 4, 9, 16, 25

```python
>>> squares = []
>>> for x in [1, 2, 3, 4, 5]:          # This is what a list comprehension does
        squares.append(x ** 2)         # Both run the iteration protocol internally
>>> squares
```
> 1, 4, 9, 16, 25

### Tuples

Tuples are sequences, like lists, but they are immutable, like strings. Functionally, they’re used to represent fixed collections of items: the components of a specific calendar date, for instance. Syntactically, they are normally coded in parentheses instead of square brackets, and they support arbitrary types, arbitrary nesting, and the usual sequence operations. The primary distinction for tuples is that they cannot be changed once created. That is, they are immutable sequences.

```python
>>> T = (1, 2, 3, 4)            # A 4-item tuple
>>> len(T)                      # Length
```
> 4

```python
>> T + (5, 6)                   # Concatenation
```
> (1, 2, 3, 4, 5, 6)

```python
>>> T[0]                        # Indexing, slicing, and more
```
> 1

```python
>>> T[0] = 2                    # Tuples are immutable
```
> TypeError: 'tuple' object does not support item assignment

Tuples are not generally used as often as lists in practice, but their immutability is the whole point. If you pass a collection of objects around your program as a list, it can be changed anywhere; if you use a tuple, it cannot. That is, tuples provide a sort of integrity constraint that is convenient in large programs.

### Files

File objects are Python code’s main interface to external files on your computer. They can be used to read and write text memos, audio clips, Excel documents, saved email messages, and whatever else you happen to have stored on your machine. For example, to create a text output file, you would pass in its name and the 'w' processing mode string to write data:

```python
>>> f = open('data.txt', 'w')      # Make a new file in output mode ('w' is write)
>>> f.write('Hello\n')             # Write strings of characters to it 
                                   # Return number of items written in Python 3.X
```
> 6

```python
>>> f.close()                      # Close to flush output buffers to disk
```

This creates a file in the current directory and writes text to it (the filename can be a full directory path if you need to access a file elsewhere on your computer). To read back what you just wrote, reopen the file in 'r' processing mode, for reading text input this is the default if you omit the mode in the call.

### Sets

Sets, for example, are a recent addition to the language that are neither mappings nor sequences; rather, they are unordered collections of unique and immutable objects. You create sets by calling the built-in set function or using new set literals and expressions.

```python
>>> X = set('spam')                 # Make a set out of a sequence in 2.X and 3.X
>>> Y = {'h', 'a', 'm'}             # Make a set with set literals in 3.X and 2.7
>>> X, Y                            # A tuple of two sets without parentheses
```
> ({'m', 'a', 'p', 's'}, {'m', 'a', 'h'})

```python
>>> X & Y                           # Intersection
```
> {'m', 'a'}

```python
>>> X | Y                           # Union
```
> {'m', 'h', 'a', 'p', 's'}

Even less mathematically inclined programmers often find sets useful for common tasks such as filtering out duplicates, isolating differences, and performing order-neutral equality tests without sorting—in lists, strings, and all other iterable objects:

```python
>>> list(set([1, 2, 1, 3, 1]))      # Filtering out duplicates (possibly reordered)
```
> 1, 2, 3

```python
>>> set('spam') - set('ham')        # Finding differences in collections
```
> {'p', 's'}

```python
>>> set('spam') == set('asmp')      # Order-neutral equality tests (== is False)
```
> True

### Type Object

The type object, returned by the type built-in function, is an object that gives the type of another object. Assuming L is a list :

```python
>>> type(L)                         # 3.X: types are classes, and vice versa
```
> <class 'list'>

```python
>>> type(type(L))                   # See Chapter 32 for more on class types
```
> <class 'type'>

Besides allowing you to explore your objects interactively, the type object in its most practical application allows code to check the types of the objects it processes. In fact, there are at least three ways to do so in a Python script:

```python
>>> if type(L) == type([]):         # Type testing, if you must...
        print('yes')
```
> yes

```python
>>> if type(L) == list:             # Using the type name
        print('yes')
```
> yes

```python
>>> if isinstance(L, list):         # Object-oriented tests
        print('yes')
```
> yes

### User-Defined Classes

Classes is an optional but powerful feature of the language that cuts development time by supporting programming by customization. Classes define new types of objects that extend the core set.

```python
>>> class Worker:
         def __init__(self, name, pay):          # Initialize when created
             self.name = name                    # self is the new object
             self.pay  = pay
         def lastName(self):
             return self.name.split()[-1]        # Split string on blanks
         def giveRaise(self, percent):
             self.pay *= (1.0 + percent)         # Update pay in place
```

Calling the class like a function generates instances of our new type, and the class’s methods automatically receive the instance being processed by a given method call.

```python
>>> bob = Worker('Bob Smith', 50000)             # Make two instances
>>> sue = Worker('Sue Jones', 60000)             # Each has name and pay attrs
>>> bob.lastName()                               # Call method: bob is self
```
> 'Smith'

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

### Q2 - Why are they called “core” data types?

```
They are known as “core” types because they are part of the Python language itself and are always 
available; to create other objects, you generally must call functions in imported modules. Most 
of the core types have specific syntax for generating the objects: 'spam', for example, is an 
expression that makes a string and deter- mines the set of operations that can be applied to it. 
Because of this, core types are hardwired into Python’s syntax. In contrast, you must call the 
built-in open function to create a file object (even though this is usually considered a core 
type too).
```

### Q3 - What does “immutable” mean, and which three of Python’s core types are considered immutable?

```
An “immutable” object is an object that cannot be changed after it is created. Numbers, strings, 
and tuples in Python fall into this category. While you cannot change an immutable object in 
place, you can always make a new one by running an expression. Bytearrays in recent Pythons 
offer mutability for text, but they are not normal strings, and only apply directly to text if 
it’s a simple 8-bit kind (e.g., ASCII).
```

### Q4 - What does “sequence” mean, and which three types fall into that category?

```
A “sequence” is a positionally ordered collection of objects. Strings, lists, and tuples are all 
sequences in Python. They share common sequence operations, such as indexing, concatenation, and 
slicing, but also have type-specific method calls. A related term, “iterable,” means either a 
physical sequence, or a virtual one that produces its items on request.
```

### Q5 - What does “mapping” mean, and which core type is a mapping?

```
The term “mapping” denotes an object that maps keys to associated values. Python’s dictionary is 
the only mapping type in the core type set. Mappings do not maintain any left-to-right positional 
ordering; they support access to data stored by key, plus type-specific method calls.
```

### Q6 - What is “polymorphism,” and why should you care?

```
“Polymorphism” means that the meaning of an operation (like a +) depends on the objects being 
operated on. This turns out to be a key idea (perhaps the key idea) behind using Python well—not 
constraining code to specific types makes that code automatically applicable to many types.
```
