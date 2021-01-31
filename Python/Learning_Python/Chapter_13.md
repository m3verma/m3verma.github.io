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

* * *

# Test Your Knowledge

### Q1 - How might you code a multiway branch in Python?

```
An if statement with multiple elif clauses is often the most straightforward way to code 
a multiway branch, though not necessarily the most concise or flexible. Dictionary 
indexing can often achieve the same result, especially if the dictionary contains callable 
functions coded with def statements or lambda expressions.
```

### Q2 - How can you code an if/else statement as an expression in Python?

```
In Python 2.5 and later, the expression form Y if X else Z returns Y if X is true, 
or Z otherwise; it’s the same as a four-line if statement. The and/or combination
(((X and Y) or Z)) can work the same way, but it’s more obscure and requires that
the Y part be true.
```

### Q3 - How can you make a single statement span many lines?

```
Wrap up the statement in an open syntactic pair ((), [], or {}), and it can span as
many lines as you like; the statement ends when Python sees the closing (right) half
of the pair, and lines 2 and beyond of the statement can begin at any indentation
level. Backslash continuations work too, but are broadly discouraged in the Python
world.
```

### Q4 - What do the words True and False mean?

```
True and False are just custom versions of the integers 1 and 0, respectively: they
always stand for Boolean true and false values in Python. They’re available for use
in truth tests and variable initialization, and are printed for expression results at
the interactive prompt. In all these roles, they serve as a more mnemonic and hence
readable alternative to 1 and 0.
```
