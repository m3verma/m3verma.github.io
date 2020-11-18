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

### Types Live with Objects, Not Variables

To see how object types come into play, watch what happens if we assign a variable multiple times:

```python
>>> a = 3             # It's an integer
>>> a = 'spam'        # Now it's a string
>>> a = 1.23          # Now it's a floating point
```

This isn’t typical Python code, but it does work—a starts out as an integer, then becomes a string, and finally becomes a floating-point number. In Python, things work more simply. Names have no types; as stated earlier, types live with objects, not names. In the preceding listing, we’ve simply changed a to reference different objects. Because variables have no type, we haven’t actually changed the type of the variable a; we’ve simply made the variable reference a different type of object.

<br>
Objects, on the other hand, know what type they are—each object contains a header field that tags the object with its type. The integer object 3, for example, will contain the value 3, plus a designator that tells Python that the object is an integer.

### Objects are Garbage-Collected

When we reassign a variable, what happens to the value it was previously referencing? For example, after the following statements, what happens to the object 3?

```python
>>> a = 3
>>> a = 'spam'
```

The answer is that in Python, whenever a name is assigned to a new object, the space held by the prior object is reclaimed if it is not referenced by any other name or object. This automatic reclamation of objects’ space is known as garbage collection. Internally, Python accomplishes this feat by keeping a counter in every object that keeps track of the number of references currently pointing to that object. As soon as (and exactly when) this counter drops to zero.

### Shared References

Now let’s introduce another variable into our interaction and watch what happens to its names and objects:

```python
>>> a = 3
>>> b = a
```

The net effect is that the variables a and b wind up referencing the same object (that is, pointing to the same chunk of memory). This scenario in Python—with multiple names referencing the same object—is usually called a shared reference (and sometimes just a shared object). Note that the names a and b are not linked to each other directly when this happens; in fact, there is no way to ever link a variable to another variable in Python. Rather, both variables point to the same object via their references.

### Shared References and In-Place Changes

There are objects and operations that perform in-place object changes—Python’s mutable types, including lists, dictionaries, and sets. For instance, an assignment to an offset in a list actually changes the list object itself in place, rather than generating a brand-new list object.

* * *

# Test Your Knowledge

### Q4 - What tools can you use to find a number’s square root, as well as its square?

```
Functions for obtaining the square root, as well as pi, tangents, and more, are available in the 
imported math module. To find a number’s square root, import math and call math.sqrt(N). To get a 
number’s square, use either the exponent expression X ** 2 or the built-in function pow(X, 2). 
Either of these last two can also compute the square root when given a power of 0.5.
```

