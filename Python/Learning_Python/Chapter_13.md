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
