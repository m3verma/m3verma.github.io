---
layout: default
---

# while and for Loops

### while Loops














### if Statements

In simple terms, the Python if statement selects actions to perform. Along with its expression counterpart, it’s the primary selection tool in Python and represents much of the logic a Python program possesses. The Python if statement is typical of if statements in most procedural languages. It takes the form of an if test, followed by one or more optional elif (“else if”) tests and a final optional else block. The tests and the else part each have an associated block of nested statements, indented under a header line. When the if statement runs, Python executes the block of code associated with the first test that evaluates to true, or the else block if all tests prove false. The general form of an if statement looks like this:

```python
if test1:            # if test
    statements1      # Associated block
elif test2:          # Optional elifs
    statements2
else:               # Optional else
    statements3
 ```
 
 ```python
>>> if 1:
...     print('true')
...
 ```
 > true
 
 ```python
>>> if not 1:
...     print('true')
... else:
...     print('false')
...
 ```
 > true
 
 Now here’s an example of a more complex if statement, with all its optional parts present :
 
 ```python
>>> x = 'killer rabbit'
>>> if x == 'roger':
...     print("shave and a haircut")
... elif x == 'bugs':
...     print("what's up doc?")
... else:
...     print('Run away! Run away!')
...
 ```
> Run away! Run away!

If you’ve used languages like C or Pascal, you might be interested to know that there is no switch or case statement in Python that selects an action based on a variable’s value. Instead, you usually code multiway branching as a series of if/elif tests, as in the prior example, and occasionally by indexing dictionaries or searching lists. Because dictionaries and lists can be built at runtime dynamically, they are sometimes more flexible than hardcoded if logic in your script :

 ```python
>>> choice = 'ham'
>>> print({'spam': 1.25,       # A dictionary-based 'switch'
...     'ham': 1.99,          # Use has_key or get for default
...     'eggs': 0.99,
...     'bacon': 1.10}[choice])
 ```
> 1.99

### Python Syntax Revisited

1. Statements execute one after another, until you say otherwise.
2. Block and statement boundaries are detected automatically.
3. Blank lines, spaces, and comments are usually ignored.
4. Docstrings are ignored but are saved and displayed by tools.

### Block Delimiters: Indentation Rules

Python detects block boundaries automatically, by line indentation—that is, the empty space to the left of your code. All statements indented the same distance to the right belong to the same block of code. In other words, the statements within a block line up vertically, as in a column. The block ends when the end of the file or a lesser-indented line is encountered, and more deeply nested blocks are simply indented further to the right than the statements in the enclosing block.

```python
x = 1
if x:
     y = 2
     if y:
         print('block2')
     print('block1')
print('block0')
```

That is, Python doesn’t care how you indent your code; it only cares that it’s done consistently. Four spaces or one tab per indentation level are common conventions, but there is no absolute standard in the Python world. Making indentation part of the syntax model also enforces consistency, a crucial component of readability in structured programming languages like Python.

### Statement Delimiters: Lines and Continuations

1. Statements may span multiple lines if you’re continuing an open syntactic pair.
2. Statements may span multiple lines if they end in a backslash.
3. Special rules for string literals - triple-quoted string.

### A Few Special Cases

 Delimited constructs, such as lists in square brackets, can span across any number of lines:
 
 ```python
L = ["Good",
     "Bad",
     "Ugly"]       # Open pairs may span lines
```

If you like using backslashes to continue lines, you can, but it’s not common practice in Python:

 ```python
if a == b and c == d and \
      d == e and f == g:
      print('olde')         # Backslashes allow continuations...
```

As another special case, Python allows you to write more than one noncompound statement (i.e., statements without nested statements) on the same line, separated by semicolons.

 ```python
x = 1; y = 2; print(x)      # More than one simple statement
```

### Truth Values and Boolean Tests

In Python :

1. All objects have an inherent Boolean true or false value.
2. Any nonzero number or nonempty object is true.
3. Zero numbers, empty objects, and the special object None are considered false.
4. Comparisons and equality tests are applied recursively to data structures.
5. Comparisons and equality tests return True or False (custom versions of 1 and 0).
6. Boolean and and or operators return a true or false operand object.
7. Boolean operators stop evaluating (“short circuit”) as soon as a result is known.

There are three Boolean expression operators in Python :

1. X and Y - Is true if both X and Y are true
2. X or Y - Is true if either X or Y is true
3. not X - Is true if X is false (the expression returns True or False)

Magnitude comparisons such as these return True or False as their truth results :

 ```python
>>> 2 < 3, 3 < 2          # Less than: return True or False (1 or 0)
```
> (True, False)

On the other hand, the and and or operators always return an object—either the object on the left side of the operator or the object on the right :

 ```python
>>> 2 or 3, 3 or 2      # Return left operand if true
```
> (2, 3)

### The if/else Ternary Expression

 Consider the following statement, which sets A to either Y or Z, based on the truth value of X :
 
```python
if X:
     A = Y
else:
    A = Z
```

Python 2.5 introduced a new expression format that allows us to say the same thing in one expression :

```python
A = Y if X else Z
```

This expression has the exact same effect as the preceding four-line if statement, but it’s simpler to code. As in the statement equivalent, Python runs expression Y only if X turns out to be true, and runs expression Z only if X turns out to be false. That is, it short-circuits, just like the Boolean operators described in the prior section, running just Y or Z but not both. Here are some examples of it in action :

```python
>>> A = 't' if 'spam' else 'f' # For strings, nonempty means true
>>> A
```
> 't'

```python
>>> A = 't' if '' else 'f'
>>> A
```
> 'f'

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
