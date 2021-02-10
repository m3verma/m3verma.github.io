---
layout: default
---

# Built-in Data Structures, Functions, and Files

## Data Structure and Sequences

### Tuple

Python’s data structures are simple but powerful. Mastering their use is a critical part of becoming a proficient Python programmer. A tuple is a fixed-length, immutable sequence of Python objects. The easiest way to create one is with a comma-separated sequence of values :

```python
In [1]: tup = 4, 5, 6
In [2]: tup
```
> (4, 5, 6)

You can convert any sequence or iterator to a tuple by invoking tuple :

```python
In [5]: tuple([4, 0, 2])
```
> (4, 0, 2)

Elements can be accessed with square brackets [] as with most other sequence types. As in C, C++, Java, and many other languages, sequences are 0-indexed in Python :

```python
In [8]: tup[0]
```
> 's'

If an object inside a tuple is mutable, such as a list, you can modify it in-place :

```python
In [11]: tup[1].append(3)
In [12]: tup
```
> ('foo', [1, 2, 3], True)

Multiplying a tuple by an integer, as with lists, has the effect of concatenating together that many copies of the tuple :

```python
In [14]: ('foo', 'bar') * 4
```
> ('foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'bar')

If you try to assign to a tuple-like expression of variables, Python will attempt to unpack the value on the righthand side of the equals sign :

```python
In [15]: tup = (4, 5, 6)
In [16]: a, b, c = tup
In [17]: b
```
> 5

A common use of variable unpacking is iterating over sequences of tuples or lists :

```python
In [27]: seq = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
In [28]: for a, b, c in seq:
   ....:     print('a={0}, b={1}, c={2}'.format(a, b, c))
```
> a=1, b=2, c=3
> a=4, b=5, c=6
> a=7, b=8, c=9

Since the size and contents of a tuple cannot be modified, it is very light on instance methods. A particularly useful one (also available on lists) is count, which counts the number of occurrences of a value :

```python
In [34]: a = (1, 2, 2, 2, 3, 4, 2)
In [35]: a.count(2)
```
> 4

### List

In contrast with tuples, lists are variable-length and their contents can be modified in-place. You can define them using square brackets [] or using the list type function :

```python
In [36]: a_list = [2, 3, 7, None]
In [37]: tup = ('foo', 'bar', 'baz')
In [38]: b_list = list(tup)
In [39]: b_list
```
> ['foo', 'bar', 'baz']

Elements can be appended to the end of the list with the append method :

```python
In [45]: b_list.append('dwarf')
In [46]: b_list
```
> ['foo', 'peekaboo', 'baz', 'dwarf']

Using insert you can insert an element at a specific location in the list :

```python
In [47]: b_list.insert(1, 'red')
In [48]: b_list
```
> ['foo', 'red', 'peekaboo', 'baz', 'dwarf']

The inverse operation to insert is pop, which removes and returns an element at a particular index :

```python
In [49]: b_list.pop(2)
```
> 'peekaboo'

```python
In [50]: b_list
```
> ['foo', 'red', 'baz', 'dwarf']

Similar to tuples, adding two lists together with + concatenates them :

```python
In [57]: [4, None, 'foo'] + [7, 8, (2, 3)]
```
> [4, None, 'foo', 7, 8, (2, 3)]

If you have a list already defined, you can append multiple elements to it using the extend method :

```python
In [58]: x = [4, None, 'foo']
In [59]: x.extend([7, 8, (2, 3)])
In [60]: x
```
> [4, None, 'foo', 7, 8, (2, 3)]

You can sort a list in-place (without creating a new object) by calling its sort function :

```python
In [61]: a = [7, 2, 5, 1, 3]
In [62]: a.sort()
In [63]: a
```
> [1, 2, 3, 5, 7]

You can select sections of most sequence types by using slice notation, which in its basic form consists of start:stop passed to the indexing operator [] :

```python
In [73]: seq = [7, 2, 3, 7, 5, 6, 0, 1]
In [74]: seq[1:5]
```
> [2, 3, 7, 5]

Negative indices slice the sequence relative to the end :

```python
In [79]: seq[-4:]
```
> [5, 6, 0, 1]

### Built-in Sequence Functions

Python has a handful of useful sequence functions that you should familiarize your‐self with and use at any opportunity. Python has a built-in function, enumerate, which returns a sequence of (i, value) tuples :

```python
for i, value in enumerate(collection):
   # do something with value
```

When you are indexing data, a helpful pattern that uses enumerate is computing a dict mapping the values of a sequence (which are assumed to be unique) to their locations in the sequence :

```python
In [83]: some_list = ['foo', 'bar', 'baz']
In [84]: mapping = {}
In [85]: for i, v in enumerate(some_list):
   ....:     mapping[v] = i
In [86]: mapping
```
> {'bar': 1, 'baz': 2, 'foo': 0}

The sorted function returns a new sorted list from the elements of any sequence :

```python
In [87]: sorted([7, 1, 2, 6, 0, 3, 2])
```
> [0, 1, 2, 2, 3, 6, 7]

zip “pairs” up the elements of a number of lists, tuples, or other sequences to create a list of tuples :

```python
In [89]: seq1 = ['foo', 'bar', 'baz']
In [90]: seq2 = ['one', 'two', 'three']
In [91]: zipped = zip(seq1, seq2)
In [92]: list(zipped)
```
> [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]

reversed iterates over the elements of a sequence in reverse order :

```python
In [100]: list(reversed(range(10)))
```
> [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

### dict

dict is likely the most important built-in Python data structure. A more common name for it is hash map or associative array. It is a flexibly sized collection of key-value pairs, where key and value are Python objects. One approach for creating one is to use curly braces {} and colons to separate keys and values :

```python
In [101]: empty_dict = {}
In [102]: d1 = {'a' : 'some value', 'b' : [1, 2, 3, 4]}
In [103]: d1
```
> {'a': 'some value', 'b': [1, 2, 3, 4]}

You can check if a dict contains a key using the same syntax used for checking whether a list or tuple contains a value :

```python
In [107]: 'b' in d1
```
> True

Since a dict is essentially a collection of 2-tuples, the dict function accepts a list of 2-tuples :

```python
In [121]: mapping = dict(zip(range(5), reversed(range(5))))
In [122]: mapping
```
> {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}

While the values of a dict can be any Python object, the keys generally have to be immutable objects like scalar types (int, float, string) or tuples (all the objects in the tuple need to be immutable, too). The technical term here is hashability. You can check whether an object is hashable (can be used as a key in a dict) with the hash function :

```python
In [127]: hash('string')
```
> 5023931463650008331

### set

A set is an unordered collection of unique elements. You can think of them like dicts, but keys only, no values. Sets support mathematical set operations like union, intersection, difference, and symmetric difference. Consider these two example sets :

```python
In [135]: a = {1, 2, 3, 4, 5}
In [136]: b = {3, 4, 5, 6, 7, 8}
In [137]: a.union(b)
```
> {1, 2, 3, 4, 5, 6, 7, 8}

```python
In [138]: a | b
```
> {1, 2, 3, 4, 5, 6, 7, 8}

All of the logical set operations have in-place counterparts, which enable you to replace the contents of the set on the left side of the operation with the result. For very large sets, this may be more efficient :

```python
In [141]: c = a.copy()
In [142]: c |= b
In [143]: c
```
> {1, 2, 3, 4, 5, 6, 7, 8}

You can also check if a set is a subset of (is contained in) or a superset of (contains all elements of) another set :

```python
In [150]: a_set = {1, 2, 3, 4, 5}
In [151]: {1, 2, 3}.issubset(a_set)
```
> True

Sets are equal if and only if their contents are equal :

```python
In [153]: {1, 2, 3} == {3, 2, 1}
```
> True

### List, Set, and Dict Comprehensions

List comprehensions are one of the most-loved Python language features. They allow you to concisely form a new list by filtering the elements of a collection, transforming the elements passing the filter in one concise expression. They take the basic form :

```python
[expr for val in collection if condition]
```

The filter condition can be omitted, leaving only the expression. For example, given a list of strings, we could filter out strings with length 2 or less and also convert them to uppercase like this :

```python
In [154]: strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
In [155]: [x.upper() for x in strings if len(x) > 2]
```
> ['BAT', 'CAR', 'DOVE', 'PYTHON']

Set and dict comprehensions are a natural extension, producing sets and dicts in an idiomatically similar way instead of lists. A dict comprehension looks like this :

```python
dict_comp = {key-expr : value-expr for value in collection
             if condition}
```

A set comprehension looks like the equivalent list comprehension except with curly braces instead of square brackets :

```python
set_comp = {expr for value in collection if condition}
```

Consider the list of strings from before. Suppose we wanted a set containing just the lengths of the strings contained in the collection; we could easily compute this using a set comprehension :

```python
In [156]: unique_lengths = {len(x) for x in strings}
In [157]: unique_lengths
```
> {1, 2, 3, 4, 6}

## Functions

Functions are the primary and most important method of code organization and reuse in Python. As a rule of thumb, if you anticipate needing to repeat the same or very similar code more than once, it may be worth writing a reusable function. Functions are declared with the def keyword and returned from with the return keyword :

```python
def my_function(x, y, z=1.5):
    if z > 1:
        return z * (x + y)
    else:
        return z / (x + y)
```

If Python reaches the end of a function without encountering a return statement, None is returned automatically. Each function can have positional arguments and keyword arguments. Keyword arguments are most commonly used to specify default values or optional arguments. In the preceding function, x and y are positional arguments while z is a keyword argument. This means that the function can be called in any of these ways :

```python
my_function(5, 6, z=0.7)
my_function(3.14, 7, 3.5)
my_function(10, 20)
```

### Namespaces, Scope, and Local Functions

Functions can access variables in two different scopes: global and local. An alternative and more descriptive name describing a variable scope in Python is a namespace. Any variables that are assigned within a function by default are assigned to the local namespace. The local namespace is created when the function is called and immediately populated by the function’s arguments. After the function is finished, the local namespace is destroyed. Consider the following function :

```python
def func():
    a = []
    for i in range(5):
        a.append(i)
```

When func() is called, the empty list a is created, five elements are appended, and then a is destroyed when the function exits.

### Returning Multiple Values

Here’s an example :

```python
def f():
    a = 5
    b = 6
    c = 7
    return a, b, c
a, b, c = f()
```

In data analysis and other scientific applications, you may find yourself doing this often. What’s happening here is that the function is actually just returning one object, namely a tuple, which is then being unpacked into the result variables. In the preceding example, we could have done this instead :

```python
return_value = f()
```

In this case, return_value would be a 3-tuple with the three returned variables. A potentially attractive alternative to returning multiple values like before might be to return a dict instead :

```python
def f():
    a = 5
    b = 6
    c = 7
    return {'a' : a, 'b' : b, 'c' : c}
```

### Anonymous (Lambda) Functions
