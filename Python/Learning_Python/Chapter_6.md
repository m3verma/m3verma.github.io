---
layout: default
---

# The Dynamic Typing Interlude

### The Case of the Missing Declaration Statements

When we type a = 3 in an interactive session or program file, for instance, how does Python know that a should stand for an integer? For that matter, how does Python know what a is at all? In Python, types are determined automatically at runtime, not in response to declarations in your code. This means that you never declare variables ahead of time.

### Variables, Objects and References

When you run an assignment statement such as a = 3 in Python, it works even if you’ve never told Python to use the name a as a variable, or that a should stand for an integer-type object. In the Python language, this all pans out in a very natural way, as follows:

1. Variable creation - A variable (i.e., name), like a, is created when your code first assigns it a value. Future assignments change the value of the already created name. Technically, Python detects some names before your code runs, but you can think of it as though initial assignments make variables.
2. Variable types - A variable never has any type information or constraints associated with it. The notion of type lives with objects, not names. Variables are generic in nature; they always simply refer to a particular object at a particular point in time.
3. Variable use - When a variable appears in an expression, it is immediately replaced with the object that it currently refers to, whatever that may be. Further, all variables must be explicitly assigned before they can be used; referencing unassigned variables results in errors.

In sum, variables are created when assigned, can reference any type of object, and must be assigned before they are referenced.

```python
>>> a = 3                # Assign a name to an object
```

Python will perform three distinct steps to carry out the request. These steps reflect the operation of all assignments in the Python language:

1. Create an object to represent the value 3.
2. Create the variable a, if it does not yet exist.
3. Link the variable a to the new object 3.

Variables and objects are stored in different parts of memory and are associated by links. Variables always link to objects and never to other variables, but larger objects may link to other objects. These links from variables to objects are called references in Python—that is, a reference is a kind of association, implemented as a pointer in memory. In concrete terms:

1. Variables are entries in a system table, with spaces for links to objects.
2. Objects are pieces of allocated memory, with enough space to represent the values for which they stand.
3. References are automatically followed pointers from variables to objects.


* * *

# Test Your Knowledge

### Q4 - What tools can you use to find a number’s square root, as well as its square?

```
Functions for obtaining the square root, as well as pi, tangents, and more, are available in the 
imported math module. To find a number’s square root, import math and call math.sqrt(N). To get a 
number’s square, use either the exponent expression X ** 2 or the built-in function pow(X, 2). 
Either of these last two can also compute the square root when given a power of 0.5.
```

