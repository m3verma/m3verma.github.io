---
layout: default
---

# Introducing Python Statements

### The python Conceptual Hierarchy Revisited

The hierarchy to the next level of python program structure :

1. Programs are composed of modules.
2. Modules contain statements.
3. Statements contain expression.
4. Expressions create and process objects.

### Python's Statements

Each statement in Python has its own specific purpose and its own specific syntax—the rules that define its structure though, as we’ll see, many share common syntax patterns, and some statements’ roles overlap. Python statements :

| Statement        | Role          | Example          |
|:-------------|:------------------|:------------------|
| Assignment | Creating references | a, b = 'good', 'bad' |
| Calls and other expressions | Running functions | log.write("spam, ham") |
| print calls | Printing objects | print('The Killer', joke) |
| if/elif/else | Selecting actions | if "python" in text:    print(text) |
| for/else | Iteration | for x in mylist:    print(x) |
| while/else | General loops | while X > Y:    print('hello') |
| pass | Empty placeholder | while True:    pass |
| break | Loop exit | while True:    if exittest(): break |
| continue | Loop continue | while True:    if skiptest(): continue |
| def | Functions and methods | def f(a, b, c=1, *d):    print(a+b+c+d[0]) |
| return | Functions results | def f(a, b, c=1, *d):    return a+b+c+d[0] |
| yield | Generator functions | def gen(n):    for i in n: yield i*2 |
| global | Namespaces  | x = 'old' def function():     global x, y; x = 'new' |
| nonlocal | Namespaces (3.X) | def outer():    x = 'old' |
| import | Module access | import sys |
| from | Attribute access | from sys import stdin |
| class | Building objects | class Subclass(Superclass):    staticData = [] |
| try/except/finally | Catching exceptions | try:    action() |
| raise | Triggering exceptions | raise EndSearch(location) |
| assert | Debugging checks | assert X > Y, 'X too small' |
| with/as | Context managers (3.X, 2.6+) | with open('data') as myfile:    process(myfile) |
| del | Deleting references | del data[k] |

### What Python Adds

The one new syntax component in Python is the colon character (:). All Python compound statements—statements that have other statements nested inside them—follow the same general pattern of a header line terminated in a colon, followed by a nested block of code usually indented underneath the header line, like this:

```python
Header line:
    Nested statement block
```

### What Python Removes

Although Python requires the extra colon character, there are three things programmers in C-like languages must include that you don’t generally have to in Python.

- Parentheses are optional

```python
if (x < y)
if x < y		#Both are equal
```

- End-of-line is end of statement - You don’t need to terminate statements with semicolons in Python the way you do in C-like languages.

```python
x = 1;
x = 1 			#Both are equal
```

- End of indentation is end of block

In Python, we consistently indent all the statements in a given single nested block the same distance to the right, and Python uses the statements’ physical indentation to determine where the block starts and stops:

```python
if x > y:
    x = 1
    y = 2
```

### Why Indentation Syntax?

The indentation rule may seem unusual at first glance to programmers accustomed to C-like languages, but it is a deliberate feature of Python, and it’s one of the main ways that Python almost forces programmers to produce uniform, regular, and readable code. To put that more strongly, aligning your code according to its logical structure is a major part of making it readable, and thus reusable and maintainable, by yourself and others. Python or otherwise, if nested blocks are not indented consistently, they become very difficult for the reader to interpret, change, or reuse, because the code no longer visually reflects its logical meaning. Readability matters, and indentation is a major component of readability.

### A Few Special Cases

Python also provides some special-purpose rules that allow customization of both statements and nested statement blocks. They’re not required and should be used sparingly, but programmers have found them useful in practice.

1. Statement rule special cases - Although statements normally appear one per line, it is possible to squeeze more than one statement onto a single line in Python by separating them with semicolons :

```python
a = 1; b = 2; print(a + b)               # Three statements on one line
```

The other special rule for statements is essentially the inverse: you can make a single statement span across multiple lines. To make this work, you simply have to enclose part of your statement in a bracketed pair—parentheses (()), square brackets ([]), or curly braces ({}). Any code enclosed in these constructs can cross multiple lines: your statement doesn’t end until Python reaches the line containing the closing part of the pair. For instance, to continue a list literal:

```python
mylist = [1111,
          2222,
          3333]
```

2. Block rule special case - As one special case here, the body of a compound statement can instead appear on the same line as the header in Python, after the colon :

```python
if x > y: print(x)
```

This allows us to code single-line if statements, single-line while and for loops, and so on. Here again, though, this will work only if the body of the compound statement itself does not contain any compound statements.

### A Quick Example : Interactive Loops

Suppose you’re asked to write a Python program that interacts with a user in a console window. In Python, typical boilerplate code for such an interactive loop might look like this :

```python
while True:
    reply = input('Enter text:')
    if reply == 'stop': break
    print(reply.upper())
```

This code makes use of a few new ideas and some we’ve already seen :
1. The code leverages the Python while loop, Python’s most general looping statement.
2. The input built-in function we met earlier in the book is used here for general console input—it prints its optional argument string as a prompt and returns the user’s typed reply as a string.
3. A single-line if statement that makes use of the special rule for nested blocks also appears here.
4. Finally, the Python break statement is used to exit the loop immediately—it simply jumps out of the loop statement altogether, and the program continues after the loop.

### Doing Math on User Inputs

Our script works, but now suppose that instead of converting a text string to uppercase, we want to do some math with numeric input—squaring it, for example, perhaps in some misguided effort of an age-input program to tease its users.

```python
>>> reply = '20'
>>> reply ** 2
```
> ...error text omitted...
> TypeError: unsupported operand type(s) for ** or pow(): 'str' and 'int'

This won’t quite work in our script, though, because Python won’t convert object types in expressions unless they are all numeric, and input from a user is always returned to our script as a string. We cannot raise a string of digits to a power unless we convert it manually to an integer :

```python
>>> int(reply) ** 2
```
> 400

### Handling Errors by Testing Inputs

So far so good, but notice what happens when the input is invalid :

```python
Enter text:xxx
...error text omitted...
```
> ValueError: invalid literal for int() with base 10: 'xxx'

The built-in int function raises an exception here in the face of a mistake. If we want our script to be robust, we can check the string’s content ahead of time with the string object’s isdigit method :

```python
>>> S = '123'
>>> T = 'xxx'
>>> S.isdigit(), T.isdigit()
```
> (True, False)

* * *

# Test Your Knowledge

### Q1 - What three things are required in a C-like language but omitted in Python?

```
C-like languages require parentheses around the tests in some statements, semicolons at 
the end of each statement, and braces around a nested block of code.
```

### Q2 - How is a statement normally terminated in Python?

```
The end of a line terminates the statement that appears on that line. Alternatively, if 
more than one statement appears on the same line, they can be terminated with semicolons; 
similarly, if a statement spans many lines, you must terminate it by closing a bracketed 
syntactic pair.
```

### Q3 - How are the statements in a nested block of code normally associated in Python?

```
The statements in a nested block are all indented the same number of tabs or spaces.
```

### Q4 - How can you make a single statement span multiple lines?

```
You can make a statement span many lines by enclosing part of it in parentheses, square 
brackets, or curly braces; the statement ends when Python sees a line that contains the 
closing part of the pair.
```

### Q5 - How can you code a compound statement on a single line?

```
The body of a compound statement can be moved to the header line after the colon, but only 
if the body consists of only noncompound statements.
```

### Q6 - Is there any valid reason to type a semicolon at the end of a statement in Python?

```
Only when you need to squeeze more than one statement onto a single line of code. Even then, 
this only works if all the statements are noncompound, and it’s discouraged because it can 
lead to code that is difficult to read.
```

### Q7 - What is a try statement for?

```
The try statement is used to catch and recover from exceptions (errors) in a Python script. 
It’s usually an alternative to manually checking for errors in your code.
```

### Q8 - What is the most common coding mistake among Python beginners?

```
Forgetting to type the colon character at the end of the header line in a compound statement 
is the most common beginner’s mistake. If you’re new to Python and haven’t made it yet, you 
probably will soon!
```
