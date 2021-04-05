---
layout: default
---

# The Documentation Interlude

### Python Documentation Sources

There are a variety of places to look for information on Python, with generally increasing verbosity. Python documentation sources :

| Form        | Role          |
|:-------------|:------------------|
| # comments | In-file documentation |
| The dir function | Lists of attributes available in objects |
| Docstrings: __doc__ | In-file documentation attached to objects |
| PyDoc: the help function | Interactive help for objects |
| PyDoc: HTML reports | Module documentation in a browser |
| Sphinx third-party tool | Richer documentation for larger projects |
| The standard manual set | Official language and library descriptions |
| Web resources | Online tutorials, examples, and so on |
| Published books | Commercially polished reference texts |

### Comments

Hash-mark comments are the most basic way to document your code. Python simply ignores all the text following a # (as long as it’s not inside a string literal), so you can follow this character with any words and descriptions meaningful to programmers. Such comments are accessible only in your source files, though; to code comments that are more widely available, you’ll need to use docstrings.

### The dir Function

The built-in dir function is an easy way to grab a list of all the attributes available inside an object (i.e., its methods and simpler data items). It can be called with no arguments to list variables in the caller’s scope. More usefully, it can also be called on any object that has attributes, including imported modules and built-in types, as well as the name of a data type.

```python
>>> import sys
>>> dir(sys)
```
> ['__displayhook__', ...more names omitted..., 'winver']

To find out what attributes are provided in objects of built-in types, run dir on a literal or an existing instance of the desired type. For example, to see list and string attributes, you can pass empty objects :

```python
>>> dir([])
```
> ['__add__', '__class__', '__contains__', ...more..., 'append', 'clear', 'copy', <br>
> 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']

The dir function serves as a sort of memory-jogger—it provides a list of attribute names, but it does not tell you anything about what those names mean. For such extra information, we need to move on to the next documentation source.

### Docstrings: __doc__

Besides # comments, Python supports documentation that is automatically attached to objects and retained at runtime for inspection. Syntactically, such comments are coded as strings at the tops of module files and function and class statements, before any other executable code. Python automatically stuffs the text of these strings, known informally as docstrings, into the __doc__ attributes of the corresponding objects. For example, consider the following file, docstrings.py. Its docstrings appear at the beginning of the file and at the start of a function and a class within it.

```python
"""
Module documentation
Words Go Here
"""
```

The whole point of this documentation protocol is that your comments are retained for inspection in __doc__ attributes after the file is imported. Thus, to display the docstrings associated with the module and its objects, we simply import the file and print their __doc__ attributes, where Python has saved the text :

```python
>>> print(docstrings.__doc__)
```
> Module documentation <br>
> Words Go Here

To fetch the docstring of a method function inside a class within a module, you would simply extend the path to go through the class: module.class.method.__doc__ . As it turns out, built-in modules and objects in Python use similar techniques to attach documentation above and beyond the attribute lists returned by dir. For example, to see an actual human-readable description of a built-in module, import it and print its __doc__ string :

```python
>>> import sys
>>> print(sys.__doc__)
```
> This module provides access to some objects used or maintained by the<br>
> interpreter and to functions that interact strongly with the interpreter.<br>
> Dynamic objects:<br>
> argv -- command line arguments; argv[0] is the script pathname if known<br>
> path -- module search path; path[0] is the script directory, else ''<br>
> modules -- dictionary of loaded modules<br>
> ...more text omitted...<br>

### PyDoc : The help Function

The standard PyDoc tool is Python code that knows how to extract docstrings and associated structural information and format them into nicely arranged reports of various types. Additional tools for extracting and formatting docstrings are available in the open source domain. There are a variety of ways to launch PyDoc, including command-line script options that can save the resulting documentation for later viewing.

```python
>>> import sys
>>> help(sys.getrefcount)
```
> Help on built-in function getrefcount in module sys:<br>
> getrefcount(...)<br>
> getrefcount(object) -> integer<br>
> Return the reference count of object.  The count returned is generally<br>
> one higher than you might expect, because it includes the (temporary)<br>
> reference as an argument to getrefcount().<br>

Besides modules, you can also use help on built-in functions, methods, and types. Usage varies slightly across Python versions, but to get help for a built-in type, try either the type name (e.g., dict for dictionary, str for string, list for list); an actual object of the type (e.g., {}, '', []); or a method of an actual object or type name (e.g., str.join, 's'.join).1 You’ll get a large display that describes all the methods available for that type or the usage of that method.

### Common Coding Gotchas

Let’s run through some of the most common mistakes beginners make when coding Python statements and programs :

1. Don’t forget the colons.
2. Start in column 1.
3. Blank lines matter at the interactive prompt.
4. Indent consistently.
5. Don’t code C in Python.
6. Use simple for loops instead of while or range.
7. Beware of mutables in assignments.
8. Don’t expect results from functions that change objects in place.
9. Always use parentheses to call a function.
10. Don’t use extensions or paths in imports and reloads.

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

