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
