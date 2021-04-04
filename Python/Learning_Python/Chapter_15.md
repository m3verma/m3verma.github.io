---
layout: default
---

# The Documentation Interlude

### Python Documentation Sources







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

