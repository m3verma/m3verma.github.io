---
layout: default
---

# Iterations and Comprehensions

### Iterations : A First Look

an object is considered iterable if it is either a physically stored sequence, or an object that produces one result at a time in the context of an iteration tool like a for loop. In a sense, iterable objects include both physical sequences and virtual sequences computed on demand.

### The Iteration Protocol: File Iterators

One of the easiest ways to understand the iteration protocol is to see how it works with a built-in type such as the file.

```python
>>> print(open('script2.py').read())
```
> import sys<br>
> print(sys.path)<br>
> x = 2<br>
> print(x ** 32)<br>

Files also have a method named __next__ in 3.X (and next in 2.X) that has a nearly identical effect—it returns the next line from a file each time it is called. The only noticeable difference is that __next__ raises a built-in StopIteration exception at end-of-file instead of returning an empty string. This interface is most of what we call the iteration protocol in Python. Any object with a __next__ method to advance to a next result, which raises StopIteration at the end of the series of results, is considered an iterator in Python. Any such object may also be stepped through with a for loop or other iteration tool, because all iteration tools normally work internally by calling __next__ on each iteration and catching the StopIteration exception to determine when to exit. The following, for example, reads a file line by line, printing the uppercase version of each line along the way, without ever explicitly reading from the file at all :

```python
>>> for line in open('script2.py'):       # Use file iterators to read by lines
...     print(line.upper(), end='')       # Calls __next__, catches StopIteration
```
> IMPORT SYS<br>
> PRINT(SYS.PATH)<br>
> X = 2<br>
> PRINT(X ** 32)<br>

This is considered the best way to read text files line by line today, for three reasons: it’s the simplest to code, might be the quickest to run, and is the best in terms of memory usage.

### Manual Iteration: iter and next

To simplify manual iteration code, Python 3.X also provides a built-in function, next, that automatically calls an object’s __next__ method.

```python
>>> f = open('script2.py')
>>> next(f)                        # The next(f) built-in calls f.__next__() in 3.X
```
> 'import sys\n'

```python
>>> next(f)                        # next(f) => [3.X: f.__next__()], [2.X: f.next()]
```
> 'print(sys.path)\n'

### The full iteration protocol

It’s really based on two objects, used in two distinct steps by iteration tools :

1. The iterable object you request iteration for, whose __iter__ is run by iter
2. The iterator object returned by the iterable that actually produces values during the iteration, whose __next__ is run by next and raises StopIteration when finished producing results.

These steps are orchestrated automatically by iteration tools in most cases, but it helps to understand these two objects’ roles. For example, in some cases these two objects are the same when only a single scan is supported (e.g., files), and the iterator object is often temporary, used internally by the iteration tool.

### Manual Iteration

Although Python iteration tools call these functions automatically, we can use them to apply the iteration protocol manually, too. The following interaction demonstrates the equivalence between automatic and manual iteration :

```python
>>> L = [1, 2, 3]
>>> for X in L:                 # Automatic iteration
...     print(X ** 2, end=' ')  # Obtains iter, calls __next__, catches exceptions
```
> 1 4 9

```python
>>> I = iter(L)                 # Manual iteration: what for loops usually do
>>> while True:
...     try:                    # try statement catches exceptions
...         X = next(I)         # Or call I.__next__ in 3.X
...     except StopIteration:
...         break
...     print(X ** 2, end=' ')
```
> 1 4 9

### Other Built-in Type Iterables

Besides files and physical sequences like lists, other types have useful iterators as well. The classic way to step through the keys of a dictionary, for example, is to request its keys list explicitly :

```python
>>> D = {'a':1, 'b':2, 'c':3}
>>> for key in D.keys():
...     print(key, D[key])
```
> a 1<br>
> b 2<br>
> c 3<br>

### List Comprehensions: A First Detailed Look

Now that we’ve seen how the iteration protocol works, let’s turn to one of its most common use cases. Together with for loops, list comprehensions are one of the most prominent contexts in which the iteration protocol is applied. we learned how to use range to change a list as we step across it :

```python
>>> L = [1, 2, 3, 4, 5]
>>> for i in range(len(L)):
...     L[i] += 10
>>> L
```
> [11, 12, 13, 14, 15]

Today, the list comprehension expression makes many such prior coding patterns obsolete. Here, for example, we can replace the loop with a single expression that produces the desired result list :

```python
>>> L = [x + 10 for x in L]
>>> L
```
> [21, 22, 23, 24, 25]

The net result is similar, but it requires less coding on our part and is likely to run substantially faster. The list comprehension isn’t exactly the same as the for loop statement version because it makes a new list object (which might matter if there are multiple references to the original list), but it’s close enough for most applications and is a common and convenient enough approach to merit a closer look here.

### List Comprehension Basics

Syntactically, its syntax is derived from a construct in set theory notation that applies an operation to each item in a set, but you don’t have to know set theory to use this tool. List comprehensions are written in square brackets because they are ultimately a way to construct a new list. They begin with an arbitrary expression that we make up, which uses a loop variable that we make up (x + 10). That is followed by what you should now recognize as the header of a for loop, which names the loop variable, and an iterable object (for x in L). To run the expression, Python executes an iteration across L inside the interpreter, assigning x to each item in turn, and collects the results of running the items through the expression on the left side. The result list we get back is exactly what the list comprehension says—a new list containing x + 10, for every x in L.

### Using List Comprehensions on Files

Let’s work through another common application of list comprehensions to explore them in more detail. Recall that the file object has a readlines method that loads the file into a list of line strings all at once :

```python
>>> f = open('script2.py')
>>> lines = f.readlines()
>>> lines
```
> ['import sys\n', 'print(sys.path)\n', 'x = 2\n', 'print(x ** 32)\n']

This works, but the lines in the result all include the newline character (\n) at the end. Anytime we start thinking about performing an operation on each item in a sequence, we’re in the realm of list comprehensions. For example, assuming the variable lines is as it was in the prior interaction, the following code does the job by running each line in the list through the string rstrip method to remove whitespace on the right side.

```python
>>> lines = [line.rstrip() for line in lines]
>>> lines
```
> ['import sys', 'print(sys.path)', 'x = 2', 'print(x ** 32)']

If we open it inside the expression, the list comprehension will automatically use the iteration protocol.

```python
>>> lines = [line.rstrip() for line in open('script2.py')]
>>> lines
```
> ['import sys', 'print(sys.path)', 'x = 2', 'print(x ** 32)']

### Extended List Comprehension Syntax

In fact, list comprehensions can be even richer in practice, and even constitute a sort of iteration mini-language in their fullest forms. As one particularly useful extension, the for loop nested in a comprehension expression can have an associated if clause to filter out of the result items for which the test is not true. For example, suppose we want to repeat the prior section’s file-scanning example, but we need to collect only lines that begin with the letter p (perhaps the first character on each line is an action code of some sort). Adding an if filter clause to our expression does the trick :

```python
>>> lines = [line.rstrip() for line in open('script2.py') if line[0] == 'p']
>>> lines
```
> ['print(sys.path)', 'print(x ** 32)']

List comprehensions can become even more complex if we need them to—for instance, they may contain nested loops, coded as a series of for clauses. In fact, their full syntax allows for any number of for clauses, each of which can have an optional associated if clause. For example, the following builds a list of the concatenation of x + y for every x in one string and every y in another. It effectively collects all the ordered combinations of the characters in two strings :

```python
>>> [x + y for x in 'abc' for y in 'lmn']
```
> ['al', 'am', 'an', 'bl', 'bm', 'bn', 'cl', 'cm', 'cn']

Beyond this complexity level, though, list comprehension expressions can often become too compact for their own good. In general, they are intended for simple types of iterations; for more involved work, a simpler for statement structure will probably be easier to understand and modify in the future. As usual in programming, if something is difficult for you to understand, it’s probably not a good idea.

### The range Iterable

In 3.X, it returns an iterable that generates numbers in the range on demand, instead of building the result list in memory.

```python
>>> R = range(10)                # range returns an iterable, not a list
>>> R
```
> range(0, 10)

```python
>>> I = iter(R)                  # Make an iterator from the range iterable
>>> next(I)                      # Advance to next result
```
> 0                                # What happens in for loops, comprehensions, etc.

```python
>>> next(I)
```
> 1

```python
>>> list(range(10))              # To force a list if required
```
> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

### The map, zip, and filter Iterables

Like range, the map, zip, and filter built-ins also become iterables in 3.X to conserve space, rather than producing a result list all at once in memory. All three not only process iterables, as in 2.X, but also return iterable results in 3.X. Unlike range, though, they are their own iterators—after you step through their results once, they are exhausted. In other words, you can’t have multiple iterators on their results that maintain different positions in those results.

```python
>>> M = map(abs, (-1, 0, 1))            # map returns an iterable, not a list
>>> M
```
> <map object at 0x00000000029B75C0>

```python
>>> next(M)                             # Use iterator manually: exhausts results
```
> 1                                       # These do not support len() or indexing

```python
>>> next(M)
```
> 0

```python
>>> next(M)
```
> 1

```python
>>> next(M)
```
> StopIteration

```python
>>> for x in M: print(x)                # map iterator is now empty: one pass only
```

The zip built-in, is an iteration context itself, but also returns an iterable with an iterator that works the same way :

```python
>>> Z = zip((1, 2, 3), (10, 20, 30))    # zip is the same: a one-pass iterator
>>> Z
```
> <zip object at 0x0000000002951108>

```python
>>> list(Z)
```
> [(1, 10), (2, 20), (3, 30)]

The filter built-in, is also analogous. It returns items in an iterable for which a passed-in function returns True (as we’ve learned, in Python True includes nonempty objects, and bool returns an object’s truth value) :

```python
>>> filter(bool, ['spam', '', 'ni'])
```
> <filter object at 0x00000000029B7B70>

```python
>>> list(filter(bool, ['spam', '', 'ni']))
```
> ['spam', 'ni']

### Multiple Versus Single Pass Iterators

It’s important to see how the range object differs from the built-ins described in this section—it supports len and indexing, it is not its own iterator (you make one with iter when iterating manually), and it supports multiple iterators over its result that remember their positions independently :

```python
>>> R = range(3)                           # range allows multiple iterators
>>> next(R)
```
> TypeError: range object is not an iterator

```python
>>> I1 = iter(R)
>>> next(I1)
```
> 0

```python
>>> next(I1)
```
> 1

```python
>>> I2 = iter(R)                           # Two iterators on one range
>>> next(I2)
```
> 0

```python
>>> next(I1)                               # I1 is at a different spot than I2
```
> 2

By contrast, in 3.X zip, map, and filter do not support multiple active iterators on the same result; because of this the iter call is optional for stepping through such objects’ results.

### Dictionary View Iterables

Finally, in Python 3.X the dictionary keys, values, and items methods return iterable view objects that generate result items one at a time, instead of producing result lists all at once in memory. Views are also available in 2.7 as an option, but under special method names to avoid impacting existing code. View items maintain the same physical ordering as that of the dictionary and reflect changes made to the underlying dictionary. Now that we know more about iterables here’s the rest of this story—in Python 3.3 (your key order may vary) :

```python
>>> D = dict(a=1, b=2, c=3)
>>> D
```
> {'a': 1, 'b': 2, 'c': 3}

```python
>>> K = D.keys()                              # A view object in 3.X, not a list
>>> K
```
> dict_keys(['a', 'b', 'c'])

```python
>>> next(K)                                   # Views are not iterators themselves
```
> TypeError: dict_keys object is not an iterator

```python
>>> I = iter(K)                               # View iterables have an iterator,
>>> next(I)                                   # which can be used manually,
```
> 'a'                                           # but does not support len(), index

```python
>>> next(I)
```
> 'b'

### Other Iteration Topics

1. User-defined functions can be turned into iterable generator functions, with yield statements.
2. List comprehensions morph into iterable generator expressions when coded in parentheses.
3. User-defined classes are made iterable with __iter__ or __getitem__ operator over-loading.

* * *

# Test Your Knowledge

### Q1 - How are for loops and iterable objects related?

```
The for loop uses the iteration protocol to step through items in the iterable object 
across which it is iterating. It first fetches an iterator from the iterable by passing
the object to iter, and then calls this iterator object’s __next__ method in 3.X on each 
iteration and catches the StopIteration exception to determine when to stop looping. 
The method is named next in 2.X, and is run by the next built-in function in both 3.x 
and 2.X. Any object that supports this model works in a for loop and in all other 
iteration contexts. For some objects that are their own iterator, the initial iter call 
is extraneous but harmless.
```

### Q2 - How are for loops and list comprehensions related?

```
Both are iteration tools and contexts. List comprehensions are a concise and often
efficient way to perform a common for loop task: collecting the results of applying an 
expression to all items in an iterable object. It’s always possible to translate a list 
comprehension to a for loop, and part of the list comprehension expression looks like 
the header of a for loop syntactically.
```

### Q3 - Name four iteration contexts in the Python language.

```
Iteration contexts in Python include the for loop; list comprehensions; the map built-
in function; the in membership test expression; and the built-in functions sorted, sum, 
any, and all. This category also includes the list and tuple built-ins, string join 
methods, and sequence assignments, all of which use the iteration protocol (see answer
#1) to step across iterable objects one item at a time.
```

### Q4 -  What is the best way to read line by line from a text file today?

```
The best way to read lines from a text file today is to not read it explicitly at all:
instead, open the file within an iteration context tool such as a for loop or list
comprehension, and let the iteration tool automatically scan one line at a time by 
running the file’s next handler method on each iteration. This approach is generally
best in terms of coding simplicity, memory space, and possibly execution speed
requirements.
```

### Q5 -  What sort of weapons would you expect to see employed by the Spanish Inquisition?

```
I’ll accept any of the following as correct answers: fear, intimidation, nice red uniforms, 
a comfy chair, and soft pillows.
```

