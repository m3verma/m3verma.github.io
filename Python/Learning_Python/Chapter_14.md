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
