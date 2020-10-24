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
