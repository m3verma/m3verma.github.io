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

```python
>>> L1 = [2, 3, 4]        # A mutable object
>>> L2 = L1               # Make a reference to the same object
>>> L1[0] = 24            # An in-place change
>>> L1                    # L1 is different
```
> 24, 3, 4

```python
>>> L2                    # But so is L2!
```
> 24, 3, 4

We haven’t changed L1 itself here; we’ve changed a component of the object that L1 references. This sort of change overwrites part of the list object’s value in place. Because the list object is shared by (referenced from) other variables, though, an inplace change like this doesn’t affect only L1—that is, you must be aware that when you make such changes, they can impact other parts of your program. It’s also just the default: if you don’t want such behavior, you can request that Python copy objects instead of making references.

```python
>>> L1 = [2, 3, 4]
>>> L2 = L1[:]            # Make a copy of L1 (or list(L1), copy.copy(L1), etc.)
>>> L1[0] = 24
>>> L1
```
> 24, 3, 4

```python
>>> L2                    # L2 is not changed
```
> 2, 3, 4

Here, the change made through L1 is not reflected in L2 because L2 references a copy of the object L1 references, not the original; that is, the two variables point to different pieces of memory.

### Shared References and Equality

Consider these statements:

```python
>>> x = 42
>>> x = 'shrubbery'       # Reclaim 42 now?
```

Because Python caches and reuses small integers and small strings, as mentioned earlier, the object 42 here is probably not literally reclaimed; instead, it will likely remain in a system table to be reused the next time you generate a 42 in your code.

```python
>>> L = [1, 2, 3]
>>> M = [1, 2, 3]         # M and L reference different objects
>>> L == M                # Same values
```
> True

```python
>>> L is M                # Different objects
```
> False

The first technique here, the == operator, tests whether the two referenced objects have the same values; this is the method almost always used for equality checks in Python. The second method, the is operator, instead tests for object identity—it returns True only if both names point to the exact same object Now, watch what happens when we perform the same operations on small numbers:

```python
>>> X = 42
>>> Y = 42                # Should be two different objects
>>> X == Y
```
> True

```python
>>> X is Y                # Same object anyhow: caching at work!
```
> True

In this interaction, X and Y should be == (same value), but not is (same object) because we ran two different literal expressions (42). Because small integers and strings are cached and reused, though, is tells us they reference the same single object. In fact, if you really want to look under the hood, you can always ask Python how many references there are to an object: the getrefcount function in the standard sys module returns the object’s reference count.

```python
>>> import sys
>>> sys.getrefcount(1)    # 647 pointers to this shared piece of memory
```
> 647


* * *

# Test Your Knowledge

### Q4 - What tools can you use to find a number’s square root, as well as its square?

```
Functions for obtaining the square root, as well as pi, tangents, and more, are available in the 
imported math module. To find a number’s square root, import math and call math.sqrt(N). To get a 
number’s square, use either the exponent expression X ** 2 or the built-in function pow(X, 2). 
Either of these last two can also compute the square root when given a power of 0.5.
```

