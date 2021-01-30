---
layout: default
---

# if Tests and Syntax Rules

### if Statements

In simple terms, the Python if statement selects actions to perform. Along with its expression counterpart, it’s the primary selection tool in Python and represents much of the logic a Python program possesses. The Python if statement is typical of if statements in most procedural languages. It takes the form of an if test, followed by one or more optional elif (“else if”) tests and a final optional else block. The tests and the else part each have an associated block of nested statements, indented under a header line. When the if statement runs, Python executes the block of code associated with the first test that evaluates to true, or the else block if all tests prove false. The general form of an if statement looks like this:

```python
if test1:            # if test
    statements1      # Associated block
elif test2:          # Optional elifs
    statements2
else:               # Optional else
    statements3
 ```
 
 ```python
>>> if 1:
...     print('true')
...
 ```
 > true
 
 ```python
>>> if not 1:
...     print('true')
... else:
...     print('false')
...
 ```
 > true
 
 Now here’s an example of a more complex if statement, with all its optional parts present :
 
 ```python
>>> x = 'killer rabbit'
>>> if x == 'roger':
...     print("shave and a haircut")
... elif x == 'bugs':
...     print("what's up doc?")
... else:
...     print('Run away! Run away!')
...
 ```
> Run away! Run away!

If you’ve used languages like C or Pascal, you might be interested to know that there is no switch or case statement in Python that selects an action based on a variable’s value. Instead, you usually code multiway branching as a series of if/elif tests, as in the prior example, and occasionally by indexing dictionaries or searching lists. Because dictionaries and lists can be built at runtime dynamically, they are sometimes more flexible than hardcoded if logic in your script :

 ```python
>>> choice = 'ham'
>>> print({'spam': 1.25,       # A dictionary-based 'switch'
...     'ham': 1.99,          # Use has_key or get for default
...     'eggs': 0.99,
...     'bacon': 1.10}[choice])
 ```
> 1.99














* * *

# Test Your Knowledge

### Q1 - Name three ways that you can assign three variables to the same value.

```
You can use multiple-target assignments (A = B = C = 0), sequence assignment 
(A, B, C = 0, 0, 0), or multiple assignment statements on three separate lines 
(A = 0, B = 0, and C = 0). 
```

### Q2 - Why might you need to care when assigning three variables to a mutable object?

```
If you assign them this way: A = B = C = [] all three names reference the same object, 
so changing it in place from one (e.g., A.append(99)) will affect the others. This is 
true only for in-place changes to mutable objects like lists and dictionaries; for 
immutable objects such as numbers and strings, this issue is irrelevant.
```

### Q3 - What’s wrong with saying L = L.sort()?

```
The list sort method is like append in that it makes an in-place change to the subject
list—it returns None, not the list it changes. The assignment back to L sets L to
None, not to the sorted list. As discussed, a newer built-in function, sorted, sorts 
any sequence and returns a new list with the sorting result; because this is not an 
in-place change, its result can be meaningfully assigned to a name.
```

### Q4 - How might you use the print operation to send text to an external file?

```
To print to a file for a single print operation, you can use 3.X’s print(X, file=F)
call form, use 2.X’s extended print >> file, X statement form, or assign sys.stdout to 
a manually opened file before the print and restore the original after. You can also 
redirect all of a program’s printed text to a file with special syntax in the system 
shell, but this is outside Python’s scope.
```
