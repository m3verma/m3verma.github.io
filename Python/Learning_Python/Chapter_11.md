---
layout: default
---

# Assignments, Expressions, and Prints

### Assignment Statements

We’ve been using the Python assignment statement for a while to assign objects to names. In its basic form, you write the target of an assignment on the left of an equals sign, and the object to be assigned on the right. The target on the left may be a name or object component, and the object on the right can be an arbitrary expression that computes an object. For the most part, assignments are straightforward, but here are a few properties to keep in mind :

1. Assignments create object references.
2. Names are created when first assigned.
3. Names must be assigned before being referenced.
4. Some operations perform assignments implicitly.

### Assignment Statement Forms

| Operation        | Role          |
|:-------------|:------------------|
| spam = 'Spam' | Basic form |
| spam, ham = 'yum', 'YUM' | Tuple assignment (positional) |
| [spam, ham] = ['yum', 'YUM'] | List assignment (positional) |
| a, b, c, d = 'spam' | Sequence assignment, generalized |
| a, *b = 'spam' | Extended sequence unpacking (Python 3.X) |
| spam = ham = 'lunch' | Multiple-target assignment |
| spams += 42 | Augmented assignment (equivalent to spams = spams + 42) |

### Sequence Assignments

Here are a few simple examples of sequence-unpacking assignments in action :

```python
>>> nudge = 1                      # Basic assignment
>>> wink  = 2
>>> A, B = nudge, wink             # Tuple assignment
>>> A, B                           # Like A = nudge; B = wink
```
> (1, 2)

```
>>> [C, D] = [nudge, wink]         # List assignment
>>> C, D
```
> (1, 2)

Python pairs the values in the tuple on the right side of the assignment operator with the variables in the tuple on the left side and assigns the values one at a time. Python creates a temporary tuple that saves the original values of the variables on the right while the statement runs, unpacking assignments are also a way to swap two variables’ values without creating a temporary variable of your own—the tuple on the right remembers the prior values of the variables automatically:

```python
>>> nudge = 1
>>> wink  = 2
>>> nudge, wink = wink, nudge      # Tuples: swaps values
>>> nudge, wink                    # Like T = nudge; nudge = wink; wink = T
```
> (2, 1)

Technically speaking, sequence assignment actually supports any iterable object on the right, not just any sequence. Although we can mix and match sequence types around the = symbol, we must generally have the same number of items on the right as we have variables on the left, or we’ll get an error.

```python
>>> string = 'SPAM'
>>> a, b, c, d = string                            # Same number on both sides
>>> a, d
```
> ('S', 'M')

```
>>> a, b, c = string                               # Error if not
```
> ...error text omitted... ValueError: too many values to unpack (expected 3)

we can even assign nested sequences, and Python unpacks their parts according to their shape, as expected.

```python
>>> ((a, b), c) = ('SP', 'AM')                     # Paired by shape and position
>>> a, b, c
```
> ('S', 'P', 'AM')

### Extended Sequence Unpacking in Python 3.X

In short, a single starred name, *X, can be used in the assignment target in order to specify a more general matching against the sequence—the starred name is assigned a list, which collects all items in the sequence not assigned to other names.

```python
>>> seq = [1, 2, 3, 4]
>>> a, *b = seq
>>> a
```
> 1

```python
>>> b
```
> [2, 3, 4]

When a starred name is used, the number of items in the target on the left need not match the length of the subject sequence. In fact, the starred name can appear anywhere in the target. For instance, in the next interaction b matches the last item in the sequence, and a matches everything before the last:

```python
>>> *a, b = seq
>>> a
```
> [1, 2, 3]

```python
>>> b
```
> 4

When the starred name appears in the middle, it collects everything between the other names listed. Thus, in the following interaction a and c are assigned the first and last items, and b gets everything in between them:

```python
>>> a, *b, c = seq
>>> a
```
> 1

```python
>>> b
```
> [2, 3]

```python
>>> c
```
> 4

Naturally, like normal sequence assignment, extended sequence unpacking syntax works for any sequence types (really, again, any iterable), not just lists. Here it is unpacking characters in a string :

```
>>> a, *b = 'spam'
>>> a, b
```
> ('s', ['p', 'a', 'm'])

Although extended sequence unpacking is flexible, some boundary cases are worth noting :

1. The starred name may match just a single item, but is always assigned a list.
2. If there is nothing left to match the starred name, it is assigned an empty list, regardless of where it appears.
3. Errors can still be triggered if there is more than one starred name, if there are too few values and no star.

Because the loop variable in the for loop statement can be any assignment target, extended sequence assignment works here too.

```python
for (a, *b, c) in [(1, 2, 3, 4), (5, 6, 7, 8)]:		# b gets [2,3] in 1st iteration
```

### Multiple-Target Assignments

A multiple-target assignment simply assigns all the given names to the object all the way to the right. The following, for example, assigns the three variables a, b, and c to the string 'spam':

```python
>>> a = b = c = 'spam'
>>> a, b, c
```
> ('spam', 'spam', 'spam')

Keep in mind that there is just one object here, shared by all three variables (they all wind up pointing to the same object in memory). This behavior is fine for immutable types—for example, when initializing a set of counters to zero (recall that variables must be assigned before they can be used in Python, so you must initialize counters to zero before you can start adding to them):

```python
>>> a = b = 0
>>> b = b + 1
>>> a, b
```
> (0, 1)

Here, changing b only changes b because numbers do not support in-place changes. As long as the object assigned is immutable, it’s irrelevant if more than one name references it.

### Augmented Assignments

They imply the combination of a binary expression and an assignment. For instance, the following two formats are roughly equivalent :

```python
X = X + Y                       # Traditional form
X += Y                          # Newer augmented form
```

Augmented assignment works on any type that supports the implied binary expression. For example, here are two ways to add 1 to a name :

```python
>>> x = 1
>>> x = x + 1                   # Traditional
>>> x
```
> 2

```python
>>> x += 1                      # Augmented
>>> x
```
> 3

Augmented assignments have three advantages :

1. There’s less for you to type.
2. The left side has to be evaluated only once. In X += Y, X may be a complicated object expression. In the augmented form, its code must be run only once. However, in the long form, X = X + Y, X appears twice and must be run twice. Because of this, augmented assignments usually run faster.
3. The optimal technique is automatically chosen. That is, for objects that support in-place changes, the augmented forms automatically perform in-place change operations instead of slower copies.

This behavior is usually what we want, but notice that it implies that the += is an inplace change for lists; thus, it is not exactly like + concatenation, which always makes a new object. As for all shared reference cases, this difference might matter if other names reference the object being changed :

```python
>>> L = [1, 2]
>>> M = L                       # L and M reference the same object
>>> L = L + [3, 4]              # Concatenation makes a new object
>>> L, M                        # Changes L but not M
```
> ([1, 2, 3, 4], [1, 2])

### Variable Name Rules

In Python, names come into existence when you assign values to them, but there are a few rules to follow when choosing names for the subjects of your programs :

1. Syntax: (underscore or letter) + (any number of letters, digits, or underscores)
2. Case matters: SPAM is not the same as spam
3. Reserved words are off-limits

Python 3.X reserved words

| False | class | finally | is | return |
| None | continue | for | lambda | try |
| True | def | from | nonlocal | while |
| and | del | global | not | with |
| as | elif | if | or | yield |
| assert | else | import | pass |
| break | except | in | raise |

Besides these rules, there is also a set of naming conventions—rules that are not required but are followed in normal practice :

1. Names that begin with a single underscore (_X) are not imported by a from module import * statement.
2. Names that have two leading and trailing underscores (__X__) are system-defined names that have special meaning to the interpreter.
3. Names that begin with two underscores and do not end with two more (__X) are localized (“mangled”) to enclosing classes.
4. The name that is just a single underscore (_) retains the result of the last expression when you are working interactively.

### Print Operations

In Python, print prints things—it’s simply a programmer-friendly interface to the standard output stream. Technically, printing converts one or more objects to their textual representations, adds some minor formatting, and sends the resulting text to either standard output or another file-like stream. The print built-in function is normally called on a line of its own, because it doesn’t return any value we care about. Because it is a normal function, though, printing in 3.X uses standard function call syntax, rather than a special statement form. And because it provides special operation modes with keyword arguments, this form is both more general and supports future enhancements better.

```python
print([object, ...][, sep=' '][, end='\n'][, file=sys.stdout][, flush=False])
```

The keyword arguments sent to this call may appear in any left-to-right order following the objects to be printed, and they control the print operation :

1. sep is a string inserted between each object’s text, which defaults to a single space if not passed; passing an empty string suppresses separators altogether.
2. end is a string added at the end of the printed text, which defaults to a \n newline character if not passed. Passing an empty string avoids dropping down to the next output line at the end of the printed text—the next print will keep adding to the end of the current output line.
3. file specifies the file, standard stream, or other file-like object to which the text will be sent; it defaults to the sys.stdout standard output stream if not passed. Any object with a file-like write(string) method may be passed, but real files should be already opened for output.
4. flush, added in 3.3, defaults to False. It allows prints to mandate that their text be flushed through the output stream immediately to any waiting recipients. Normally, whether printed output is buffered in memory or not is determined by file; passing a true value to flush forcibly flushes the stream.

The following prints a variety of object types to the default standard output stream, with the default separator and end-of-line formatting added :

```python
>>> print()                                      # Display a blank line
>>> x = 'spam'
>>> y = 99
>>> z = ['eggs']
>>>
>>> print(x, y, z)                               # Print three objects per defaults
```
> spam 99 ['eggs']

By default, print calls add a space between the objects printed. To suppress this, send an empty string to the sep keyword argument, or send an alternative separator of your choosing:

```python
>>> print(x, y, z, sep='')                       # Suppress separator
```
> spam99['eggs']

```python
>>> print(x, y, z, sep=', ')                     # Custom separator
```
> spam, 99, ['eggs']

Also by default, print adds an end-of-line character to terminate the output line. You can suppress this and avoid the line break altogether by passing an empty string to the end keyword argument :

```python
>>> print(x, y, z, end='')                        # Suppress line break
```
> spam 99 ['eggs']>>>

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
