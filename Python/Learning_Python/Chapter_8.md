---
layout: default
---

# Lists and Dictionaries

### Lists

Lists are Python’s most flexible ordered collection object type. Unlike strings, lists can contain any sort of object: numbers, strings, and even other lists. Also, unlike strings, lists may be changed in place by assignment to offsets and slices, list method calls, deletion statements, and more—they are mutable objects.

Python lists are :
1. Ordered collections of arbitrary objects.
2. Accessed by offset.
3. Variable-length, heterogeneous, and arbitrarily nestable.
4. Of the category “mutable sequence”.
5. Arrays of object references

Common list literals and operations :

| Operation        | Interpretation          |
|:-------------|:------------------|
| `L = []`          | Empty list |
| `L = [123, 'abc', 1.23, {}]` | Four items: indexes 0..3 |
| `L = ['Bob', 40.0, ['dev', 'mgr']]` | Nested sublists |
| L = list('spam') | List of an iterable’s items |
| `L[i]` | Index |
| L1 + L2 | Concatenate |
| for x in L: print(x) | Iteration |

### Lists in Action

Because they are sequences, lists support many of the same operations as strings. For example, lists respond to the + and * operators much like strings—they mean concatenation and repetition here too, except that the result is a new list, not a string :

```python
>>> len([1, 2, 3])                           # Length
```
> 3

```python
>>> [1, 2, 3] + [4, 5, 6]                    # Concatenation
```
> 1, 2, 3, 4, 5, 6

```python
>>> ['Ni!'] * 4                              # Repetition
```
> 'Ni!', 'Ni!', 'Ni!', 'Ni!'

Although the + operator works the same for lists and strings, it’s important to know that it expects the same sort of sequence on both sides—otherwise, you get a type error when the code runs. More generally, lists respond to all the sequence operations we used on strings in the prior chapter, including iteration tools:

```python
>>> 3 in [1, 2, 3]                           # Membership
```
> True

```python
>>> for x in [1, 2, 3]:
...     print(x, end=' ')                    # Iteration (2.X uses: print x,)
...
```
> 1 2 3

Because lists are sequences, indexing and slicing work the same way for lists as they do for strings. However, the result of indexing a list is whatever type of object lives at the offset you specify, while slicing a list always returns a new list :

```python
>>> L = ['spam', 'Spam', 'SPAM!']
>>> L[2]                              # Offsets start at zero
```
> 'SPAM!'

```python
>>> L[−2]                             # Negative: count from the right
```
> 'Spam'

```python
>>> L[1:]                             # Slicing fetches sections
```
> ['Spam', 'SPAM!']

### Changing Lists in Place

Because lists are mutable, they support operations that change a list object in place. That is, the operations in this section all modify the list object directly—overwriting its former value—without requiring that you make a new copy, as you had to for strings.

```python
>>> L = ['spam', 'Spam', 'SPAM!']
>>> L[1] = 'eggs'                     # Index assignment
>>> L
```
> ['spam', 'eggs', 'SPAM!']

```python
>>> L[0:2] = ['eat', 'more']          # Slice assignment: delete+insert
>>> L                                 # Replaces items 0,1
```
> ['eat', 'more', 'SPAM!']

Both index and slice assignments are in-place changes—they modify the subject list directly, rather than generating a new list object for the result. Because the length of the sequence being assigned does not have to match the length of the slice being assigned to, slice assignment can be used to replace (by overwriting), expand (by inserting), or shrink (by deleting) the subject list.

```python
>>> L = [1, 2, 3]
>>> L[1:2] = [4, 5]                   # Replacement/insertion
>>> L
```
> [1, 4, 5, 3]

```python
>>> L[1:1] = [6, 7]                   # Insertion (replace nothing)
>>> L
```
> [1, 6, 7, 4, 5, 3]

```python
>>> L[1:2] = []                       # Deletion (insert nothing)
>>> L
```
> [1, 7, 4, 5, 3]

Like strings, Python list objects also support type-specific method calls, many of which change the subject list in place:

```python
>>> L = ['eat', 'more', 'SPAM!']
>>> L.append('please')                # Append method call: add item at end
>>> L
```
> ['eat', 'more', 'SPAM!', 'please']

```python
>>> L.sort()                          # Sort list items ('S' < 'e')
>>> L
```
> ['SPAM!', 'eat', 'more', 'please']

You can modify sort behavior by passing in keyword arguments—a special “name=value” syntax in function calls that specifies passing by name and is often used for giving configuration options. In sorts, the reverse argument allows sorts to be made in descending instead of ascending order, and the key argument gives a one-argument function that returns the value to be used in sorting.

```python
>>> L = ['abc', 'ABD', 'aBe']
>>> L.sort()                                # Sort with mixed case
>>> L
```
> ['ABD', 'aBe', 'abc']

```python
>>> L = ['abc', 'ABD', 'aBe']
>>> L.sort(key=str.lower)                   # Normalize to lowercase
>>> L
```
> ['abc', 'ABD', 'aBe']

```python
>>> L = ['abc', 'ABD', 'aBe']
>>> L.sort(key=str.lower, reverse=True)     # Change sort order
>>> L
```
> ['aBe', 'ABD', 'abc']

One warning here: beware that append and sort change the associated list object in place, but don’t return the list as a result. Pythons as a built-in function, which sorts any collection (not just lists) and returns a new list for the result :

```python
>>> L = ['abc', 'ABD', 'aBe']
>>> sorted(L, key=str.lower, reverse=True)          # Sorting built-in
```
> ['aBe', 'ABD', 'abc']

Reverse reverses the list in-place, and the extend and pop methods insert multiple items at and delete an item from the end of the list, respectively.

```python
>>> L = [1, 2]
>>> L.extend([3, 4, 5])              # Add many items at end (like in-place +)
>>> L
```
> [1, 2, 3, 4, 5]

```python
>>> L.pop()                          # Delete and return last item (by default: −1)
```
> 5

```python
>>> L
```
> [1, 2, 3, 4]

```python
>>> L.reverse()                      # In-place reversal method
>>> L
```
> [4, 3, 2, 1]

The list pop method is often used in conjunction with append to implement a quick last-in-first-out (LIFO) stack structure. The end of the list serves as the top of the stack :

```python
>>> L = []
>>> L.append(1)                      # Push onto stack
>>> L.append(2)
>>> L
```
> [1, 2]

```python
>>> L.pop()                          # Pop off stack
```
> 2

```python
>>> L
```
> [1]

The pop method also accepts an optional offset of the item to be deleted and returned (the default is the last item at offset −1). Because lists are mutable, you can use the del statement to delete an item or section in place :

```python
>>> L = ['spam', 'eggs', 'ham', 'toast']
>>> del L[0]                         # Delete one item
>>> L
```
> ['eggs', 'ham', 'toast']

```
>>> del L[1:]                        # Delete an entire section
>>> L                                # Same as L[1:] = []
```
> ['eggs']

### Dictionaries

Along with lists, dictionaries are one of the most flexible built-in data types in Python. Dictionaries also sometimes do the work of records, structs, and symbol tables used in other languages; can be used to represent sparse (mostly empty) data structures; and much more. Here’s a rundown of their main properties. Python dictionaries are:

1. Accessed by key, not offset position.
2. Unordered collections of arbitrary objects.
3. Variable-length, heterogeneous, and arbitrarily nestable.
4. Of the category “mutable mapping”
5. Tables of object references (hash tables).

Common dictionary literals and operations :

| Operation        | Interpretation          |
|:-------------|:------------------|
| D = {}         | Empty dictionary |
| D = {'name': 'Bob', 'age': 40} | Two-item dictionary |
| E = {'cto': {'name': 'Bob', 'age': 40}} | Nesting |
| D = dict(name='Bob', age=40) | Alternative construction techniques |
| D = dict([('name', 'Bob'), ('age', 40)]) | Keywords |
| D['name'] | Indexing by key |
| 'age' in D | Membership |
| D.keys() | Methods: all keys, |
| D.values() | all values, |
| D.items() | all key+value tuples, |
| D.copy() | copy (top-level), |
| D.clear() | clear (remove all items), |
| D.update(D2) | merge by keys, |
| D.get(key, default?) | fetch by key, if absent default (or None), |
| D.pop(key, default?) | remove by key, if absent default (or error) |
| D.setdefault(key, default?) | fetch by key, if absent set default (or None), |
| D.popitem() | remove/return any (key, value) pair; etc. |
| len(D) | Length: number of stored entries |
| D[key] = 42 | Adding/changing keys |
| del D[key] | Deleting entries by key |

### Dictionaries in Action

When Python creates a dictionary, it stores its items in any left-to-right order it chooses; to fetch a value back, you supply the key with which it is associated, not its relative position. In normal operation, you create dictionaries with literals and store and access items by key with indexing :

```python
>>> D = {'spam': 2, 'ham': 1, 'eggs': 3}      # Make a dictionary
>>> D['spam']                                 # Fetch a value by key
```
> 2

```python
>>> D                                         # Order is "scrambled"
```
> {'eggs': 3, 'spam': 2, 'ham': 1}

Here, the dictionary is assigned to the variable D; the value of the key 'spam' is the integer 2, and so on. The left-to-right order of keys in a dictionary will almost always be different from what you originally typed. This is on purpose: to implement fast key lookup (a.k.a. hashing), keys need to be reordered in memory. The built-in len function works on dictionaries, too; it returns the number of items stored in the dictionary or, equivalently, the length of its keys list. The dictionary in membership operator allows you to test for key existence, and the keys method returns all the keys in the dictionary.

```python
>>> len(D)                                    # Number of entries in dictionary
```
> 3

```python
>>> 'ham' in D                                # Key membership test alternative
```
> True

```python
>>> list(D.keys())                            # Create a new list of D's keys
```
> ['eggs', 'spam', 'ham']

### Changing Dictionaries in Place

Dictionaries, like lists, are mutable, so you can change, expand, and shrink them in place without making new dictionaries: simply assign a value to a key to change or create an entry.

```python
>>> D
```
> {'eggs': 3, 'spam': 2, 'ham': 1}

```python
>>> D['ham'] = ['grill', 'bake', 'fry']           # Change entry (value=list)
>>> D
```
> {'eggs': 3, 'spam': 2, 'ham': ['grill', 'bake', 'fry']}

```python
>>> del D['eggs']                                 # Delete entry
>>> D
```
> {'spam': 2, 'ham': ['grill', 'bake', 'fry']}

```python
>>> D['brunch'] = 'Bacon'                         # Add new entry
>>> D
```
> {'brunch': 'Bacon', 'spam': 2, 'ham': ['grill', 'bake', 'fry']}

Dictionary methods provide a variety of type-specific tools. For instance, the dictionary values and items methods return all of the dictionary’s values and (key,value) pair tuples, respectively; along with keys, these are useful in loops that need to step through dictionary entries one by one.

```python
>>> D = {'spam': 2, 'ham': 1, 'eggs': 3}
>>> list(D.values())
```
> [3, 2, 1]

```python
>>> list(D.items())
```
> ('eggs', 3), ('spam', 2), ('ham', 1)

Fetching a nonexistent key is normally an error, but the get method returns a default value—None, or a passed-in default—if the key doesn’t exist.

```python
>>> D.get('spam')                          # A key that is there
```
> 2

```python
>>> print(D.get('toast'))                  # A key that is missing
```
> None

```python
>>> D.get('toast', 88)
```
> 88

Finally, the dictionary pop method deletes a key from a dictionary and returns the value it had. It’s similar to the list pop method, but it takes a key instead of an optional position :

```python
>>> D
```
> {'eggs': 3, 'muffin': 5, 'toast': 4, 'spam': 2, 'ham': 1}

```python
>>> D.pop('muffin')
```
> 5

```python
>>> D.pop('toast')                         # Delete and return from a key
```
> 4

```python
>>> D
```
> {'eggs': 3, 'spam': 2, 'ham': 1}

```python
>>> L = ['aa', 'bb', 'cc', 'dd']
>>> L.pop()                                # Delete and return from the end
```
> 'dd'

### Example : Movie Database

Let’s look at a more realistic dictionary example. In honor of Python’s namesake, the following example creates a simple in-memory Monty Python movie database, as a table that maps movie release date years (the keys) to movie titles (the values). As coded, you fetch movie names by indexing on release year strings :

```python
>>> table = {'1975': 'Holy Grail',              # Key: Value
...          '1979': 'Life of Brian',
...          '1983': 'The Meaning of Life'}
>>>
>>> year  = '1983'
>>> movie = table[year]                         # dictionary[Key] => Value
>>> movie
```
> 'The Meaning of Life'

### Dictionary Usage Notes

Dictionaries are fairly straightforward tools once you get the hang of them, but here are a few additional pointers and reminders you should be aware of when using them :

1. Sequence operations don’t work - Dictionaries are mappings, not sequences.
2. Assigning to new indexes adds entries - Keys can be created when you write a dictionary literal (embedded in the code of the literal itself), or when you assign values to new keys of an existing dictionary object individually. The end result is the same.
3. Keys need not always be strings. Our examples so far have used strings as keys, but any other immutable objects work just as well.

### Nesting in Dictionaries

The following again uses a dictionary to capture object properties, but it codes it all at once (rather than assigning to each key separately) and nests a list and a dictionary to represent structured property values :

```python
>>> rec = {'name': 'Bob',
...        'jobs': ['developer', 'manager'],
...        'web':  'www.bobs.org/˜Bob',
...        'home': {'state': 'Overworked', 'zip': 12345}}
```

To fetch components of nested objects, simply string together indexing operations :

```python
>>> rec['name']
```
> 'Bob'

```python
>>> rec['jobs']
```
> ['developer', 'manager']

```python
>>> rec['jobs'][1]
```
> 'manager'

```python
>>> rec['home']['zip']
```
> 12345

### Other Ways to Make Dictionaries

Finally, note that because dictionaries are so useful, more ways to build them have emerged over time. In Python 2.3 and later, for example, the last two calls to the dict constructor (really, type name) shown here have the same effect as the literal and keyassignment forms above them :

```python
{'name': 'Bob', 'age': 40}             # Traditional literal expression
D = {}                                 # Assign by keys dynamically
D['name'] = 'Bob'
D['age']  = 40
dict(name='Bob', age=40)               # dict keyword argument form
dict([('name', 'Bob'), ('age', 40)])   # dict key/value tuples form
```

All four of these forms create the same two-key dictionary, but they are useful in differing circumstances :

1. The first is handy if you can spell out the entire dictionary ahead of time.
2. The second is of use if you need to create the dictionary one field at a time on the fly.
3. The third involves less typing than the first, but it requires all keys to be strings.
4. The last is useful if you need to build up keys and values as sequences at runtime.

* * *

# Test Your Knowledge

### Q1 - Can the string find method be used to search a list?

```
No, because methods are always type-specific; that is, they only work on a single data type. 
Expressions like X+Y and built-in functions like len(X) are generic, though, and may work on a 
variety of types. In this case, for instance, the in membership expression has a similar effect 
as the string find, but it can be used to search both strings and lists. In Python 3.X, there 
is some attempt to group methods by categories (for example, the mutable sequence types list 
and bytearray have similar method sets), but methods are still more type-specific than other 
operation sets.
```

