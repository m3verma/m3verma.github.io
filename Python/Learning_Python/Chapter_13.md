---
layout: default
---

# while and for Loops

### while Loops

Python’s while statement is the most general iteration construct in the language. In simple terms, it repeatedly executes a block of (normally indented) statements as long as a test at the top keeps evaluating to a true value. It is called a “loop” because control keeps looping back to the start of the statement until the test becomes false. When the test becomes false, control passes to the statement that follows the while block.

```python
while test:         # Loop test
    statements      # Loop body
else:               # Optional else
    statements      # Run if didn't exit loop with break
```

To illustrate, let’s look at a few simple while loops in action. The first, which consists of a print statement nested in a while loop, just prints a message forever.

```python
>>> while True:
...     print('Type Ctrl-C to stop me!')
```

The next example keeps slicing off the first character of a string until the string is empty and hence false.

```python
>>> x = 'spam'
>>> while x:                    # While x is not empty
...     print(x, end=' ')       # In 2.X use print x,
...     x = x[1:]               # Strip first character off x
...
```
> spam pam am m

### break, continue, pass, and the Loop else

In Python :

1. break - Jumps out of the closest enclosing loop (past the entire loop statement)
2. continue - Jumps to the top of the closest enclosing loop (to the loop’s header line)
3. pass - Does nothing at all: it’s an empty statement placeholder
4. Loop else block - Runs if and only if the loop is exited normally (i.e., without hitting a break)

Factoring in break and continue statements, the general format of the while loop looks like this :

```python
while test:
    statements
    if test: break          # Exit loop now, skip else if present
    if test: continue       # Go to top of loop now, to test1
else:
    statements              # Run if we didn't hit a 'break'
```

Simple things first: the pass statement is a no-operation placeholder that is used when the syntax requires a statement, but you have nothing useful to say. It is often used to code an empty body for a compound statement. For instance, if you want to code an infinite loop that does nothing each time through, do it with a pass : 

```python
while True: pass        # Type Ctrl-C to stop me!
```

The continue statement causes an immediate jump to the top of a loop. It also sometimes lets you avoid statement nesting. The next example uses continue to skip odd numbers.

```python
x = 10
while x:
    x = x−1                     # Or, x -= 1
    if x % 2 != 0: continue     # Odd? -- skip print
    print(x, end=' ')
```

Python has no “go to” statement, but because continue lets you jump about in a program, many of the warnings about readability and maintainability you may have heard about “go to” apply. The break statement causes an immediate exit from a loop. Because the code that follows it in the loop is not executed if the break is reached, you can also sometimes avoid nesting by including a break.

```python
>>> while True:
...     name = input('Enter name:')                     # Use raw_input() in 2.X
...     if name == 'stop': break
...     age = input('Enter age: ')
...     print('Hello', name, '=>', int(age) ** 2)
```
> Enter name:bob
> Enter age: 40
> Hello bob => 1600
> Enter name:sue
> Enter age: 30
> Hello sue => 900
> Enter name:stop

When combined with the loop else clause, the break statement can often eliminate the need for the search status flags used in other languages. For instance, the following piece of code determines whether a positive integer y is prime by searching for factors greater than 1 :

```python
x = y // 2 # For some y > 1
while x > 1:
    if y % x == 0: # Remainder
        print(y, 'has factor', x)
        break # Skip else
    x -= 1
else: # Normal exit
    print(y, 'is prime')
```

The loop else clause is also run if the body of the loop is never executed, as you don’t run a break in that event either; in a while loop, this happens if the test in the header is false to begin with.

### for Loops

The for loop is a generic iterator in Python: it can step through the items in any ordered sequence or other iterable object. The for statement works on strings, lists, tuples, and other built-in iterables, as well as new user-defined objects. The Python for loop begins with a header line that specifies an assignment target (or targets), along with the object you want to step through. The header is followed by a block of (normally indented) statements that you want to repeat :

```python
for target in object:                   # Assign object items to target
    statements                          # Repeated loop body: use target
else:                                   # Optional else part
    statements                          # If we didn't hit a 'break'
```

The for statement also supports an optional else block, which works exactly as it does in a while loop—it’s executed if the loop exits without running into a break statement :

```python
for target in object:       # Assign object items to target
    statements
    if test: break          # Exit loop now, skip else
    if test: continue       # Go to top of loop now
else:
    statements              # If we didn't hit a 'break'
```

A for loop can step across any kind of sequence object. In our first example, for instance, we’ll assign the name x to each of the three items in a list in turn, from left to right, and the print statement will be executed for each.

```python
>>> for x in ["spam", "eggs", "ham"]:
...     print(x, end=' ')
```
> spam eggs ham

Any sequence works in a for, as it’s a generic tool. For example, for loops work on strings and tuples :

```python
>>> S = "lumberjack"
>>> T = ("and", "I'm", "okay")
>>> for x in S: print(x, end=' ')       # Iterate over a string
...
```
> l u m b e r j a c k

```python
>>> for x in T: print(x, end=' ')       # Iterate over a tuple
...
```
> and I'm okay

Tuples in for loops also come in handy to iterate through both keys and values in dictionaries using the items method, rather than looping through the keys and indexing to fetch the values manually :

```python
>>> D = {'a': 1, 'b': 2, 'c': 3}
>>> for key in D:
...     print(key, '=>', D[key]) # Use dict keys iterator and index
...
```
> a => 1
> c => 3
> b => 2

```python
>>> for (a, *b, c) in [(1, 2, 3, 4), (5, 6, 7, 8)]:
... print(a, b, c)
...
```
> 1 [2, 3] 4
> 5 [6, 7] 8

The next example illustrates statement nesting and the loop else clause in a for. Given a list of objects (items) and a list of keys (tests), this code searches for each key in the objects list and reports on the search’s outcome :

```python
>>> items = ["aaa", 111, (4, 5), 2.01]          # A set of objects
>>> tests = [(4, 5), 3.14]                      # Keys to search for
>>>
>>> for key in tests:                           # For all keys
...     for item in items:                      # For all items
...         if item == key:                     # Check for match
...             print(key, "was found")
...             break
...     else:
...         print(key, "not found!")
...
```
> (4, 5) was found
> 3.14 not found!

### Loop Coding Techniques

You can always code such unique iterations with a while loop and manual indexing, but Python provides a set of built-ins that allow you to specialize the iteration in a for :

1. The built-in range function (available since Python 0.X) produces a series of successively higher integers, which can be used as indexes in a for.
2. The built-in zip function (available since Python 2.0) returns a series of parallelitem tuples, which can be used to traverse multiple sequences in a for.
3. The built-in enumerate function (available since Python 2.3) generates both the values and indexes of items in an iterable, so we don’t need to count manually.
4. The built-in map function (available since Python 1.0) can have a similar effect to zip in Python 2.X, though this role is removed in 3.X.

Because for loops may run quicker than while-based counter loops, though, it’s to your advantage to use tools like these that allow you to use for whenever possible. Our first loop-related function, range, is really a general tool that can be used in a variety of contexts. Although it’s used most often to generate indexes in a for, you can use it anywhere you need a series of integers. In Python 2.X range creates a physical list; in 3.X, range is an iterable that generates items on demand, so we need to wrap it in a list call to display its results all at once in 3.X only :

```python
>>> list(range(5)), list(range(2, 5)), list(range(0, 10, 2))
```
> ([0, 1, 2, 3, 4], [2, 3, 4], [0, 2, 4, 6, 8])

As a general rule, use for instead of while whenever possible, and don’t use range calls in for loops except as a last resort. The built-in zip function allows us to use for loops to visit multiple sequences in parallel—not overlapping in time, but during the same loop. In basic operation, zip takes one or more sequences as arguments and returns a series of tuples that pair up parallel items taken from those sequences.

```python
>>> L1 = [1,2,3,4]
>>> L2 = [5,6,7,8]
```

```python
>>> zip(L1, L2)
```
> <zip object at 0x026523C8>

```python
>>> list(zip(L1, L2)) # list() required in 3.X, not 2.X
```
> [(1, 5), (2, 6), (3, 7), (4, 8)]

```python
>>> for (x, y) in zip(L1, L2):
... print(x, y, '--', x+y)
...
```
> 1 5 -- 6
> 2 6 -- 8
> 3 7 -- 10
> 4 8 -- 12

zip truncates result tuples at the length of the shortest sequence when the argument lengths differ. In the following, we zip together two strings to pick out characters in parallel, but the result has only as many tuples as the length of the shortest sequence :

```python
>>> S1 = 'abc'
>>> S2 = 'xyz123'
>>>
>>> list(zip(S1, S2)) # Truncates at len(shortest)
```
> [('a', 'x'), ('b', 'y'), ('c', 'z')]

The enumerate function returns a generator object— In short, it has a method called by the next built-in function, which returns an (index, value) tuple each time through the loop. The for steps through these tuples automatically, which allows us to unpack their values with tuple assignment, much as we did for zip :

```python
>>> E = enumerate(S)
>>> E
```
> <enumerate object at 0x0000000002A8B900>

```python
>>> next(E)
```
> (0, 's')

```python
>>> next(E)
```
> (1, 'p')

```python
>>> next(E)
```
> (2, 'a')


* * *

# Test Your Knowledge

### Q1 - What are the main functional differences between a while and a for?

```
The while loop is a general looping statement, but the for is designed to iterate
across items in a sequence or other iterable. Although the while can imitate the
for with counter loops, it takes more code and might run slower
```

### Q2 - What’s the difference between break and continue?

```
The break statement exits a loop immediately (you wind up below the entire
while or for loop statement), and continue jumps back to the top of the loop (you
wind up positioned just before the test in while or the next item fetch in for).
```

### Q3 - When is a loop’s else clause executed?

```
The else clause in a while or for loop will be run once as the loop is exiting, if the
loop exits normally (without running into a break statement). A break exits the
loop immediately, skipping the else part on the way out (if there is one).
```

### Q4 -  How can you code a counter-based loop in Python?

```
Counter loops can be coded with a while statement that keeps track of the index
manually, or with a for loop that uses the range built-in function to generate 
successive integer offsets. Neither is the preferred way to work in Python, if you need
to simply step across all the items in a sequence. Instead, use a simple for loop
instead, without range or counters, whenever possible; it will be easier to code and
usually quicker to run.
```

### Q5 -  What can a range be used for in a for loop?

```
The range built-in can be used in a for to implement a fixed number of repetitions,
to scan by offsets instead of items at offsets, to skip successive items as you go, and
to change a list while stepping across it. None of these roles requires range, and
most have alternatives—scanning actual items, three-limit slices, and list comprehensions 
are often better solutions today.
```
