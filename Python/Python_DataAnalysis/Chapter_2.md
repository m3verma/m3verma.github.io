---
layout: default
---

# Python Language Basics, IPython, and Jupyter Notebooks

## The Python Interpreter

Python is an interpreted language. The Python interpreter runs a program by executing one statement at a time. The standard interactive Python interpreter can be invoked on the command line with the python command. Running Python programs is as simple as calling python with a .py file as its first argument. Suppose we had created hello_world.py with these contents :

```python
print('Hello world')
```

You can run it by executing the following command (the hello_world.py file must be in your current working terminal directory) :

```python
$ python hello_world.py
```
> Hello world

## IPython Basics

### Running the IPython Shell

You can launch the IPython shell on the command line just like launching the regular Python interpreter except with the ipython command:

```python
$ ipython
```

You can execute arbitrary Python statements by typing them in and pressing Return (or Enter). When you type just a variable into IPython, it renders a string representation of the object :

```python
In [5]: import numpy as np
```

IPython also provides facilities to execute arbitrary blocks of code (via a somewhat glorified copy-and-paste approach) and whole Python scripts. You can also use the Jupyter notebook to work with larger blocks of code.

### Running the Jupyter Notebook

One of the major components of the Jupyter project is the notebook, a type of interactive document for code, text (with or without markup), data visualizations, and other output. On many platforms, Jupyter will automatically open up in your default web browser (unless you start it with --no-browser). Otherwise, you can navigate to the HTTP address printed when you started the notebook, here http://localhost:8888/. To create a new notebook, click the New button and select the “Python 3” or “conda [default]” option. When you save the notebook (see “Save and Checkpoint” under the notebook File menu), it creates a file with the extension .ipynb. This is a self-contained file format that contains all of the content (including any evaluated code output) currently in the notebook. These can be loaded and edited by other Jupyter users.

### Tab Completion

One of the major improvements over the standard Python shell is tab completion, found in many IDEs or other interactive computing analysis environments. While entering expressions in the shell, pressing the Tab key will search the namespace for any variables (objects, functions, etc.) matching the characters you have typed so far :

```python
In [1]: an_apple = 27
In [2]: an_example = 42
In [3]: an<Tab>
```
> an_apple    and         an_example  any


In this example, note that IPython displayed both the two variables I defined as well as the Python keyword and and built-in function any. Naturally, you can also complete methods and attributes on any object after typing a period.

```python
In [3]: b = [1, 2, 3]
In [4]: b.<Tab>
```
> b.append  b.count   b.insert  b.reverse
> b.clear   b.extend  b.pop     b.sort
> b.copy    b.index   b.remove

### Introspection

Using a question mark (?) before or after a variable will display some general information about the object :

```python
In [8]: b = [1, 2, 3]
In [9]: b?
```
> Type:       list
> String Form:[1, 2, 3]
> Length:     3
> Docstring:
> list() -> new empty list
> list(iterable) -> new list initialized from iterable's items

This is referred to as object introspection. If the object is a function or instance method, the docstring, if defined, will also be shown. Using ?? will also show the function’s source code if possible :

### The %run Command

You can run any file as a Python program inside the environment of your IPython session using the %run command. Suppose you had the following simple script stored in ipython_script_test.py :

```python
def f(x, y, z):
    return (x + y) / z
a = 5
b = 6
c = 7.5
result = f(a, b, c)
```

You can execute this by passing the filename to %run:

```python
In [14]: %run ipython_script_test.py
```

The script is run in an empty namespace (with no imports or other variables defined) so that the behavior should be identical to running the program on the command line using python script.py. All of the variables (imports, functions, and globals) defined in the file (up until an exception, if any, is raised) will then be accessible in the IPython shell. In the Jupyter notebook, you may also use the related %load magic function, which imports a script into a code cell :

```python
>>> %load ipython_script_test.py
```

Pressing Ctrl-C while any code is running, whether a script through %run or a long-running command, will cause a KeyboardInterrupt to be raised. This will cause nearly all Python programs to stop immediately except in certain unusual cases.

### Executing Code from the Clipboard

The most foolproof methods are the %paste and %cpaste magic functions. %paste takes whatever text is in the clipboard and executes it as a single block in the shell :

```python
In [17]: %paste
x = 5
y = 7
if x > 5:
    x += 1

    y = 8
## -- End pasted text --
```

%cpaste is similar, except that it gives you a special prompt for pasting code into :

```python
In [18]: %cpaste
Pasting code; enter '--' alone on the line to stop or use Ctrl-D.
:x = 5
:y = 7
:if x > 5:
:    x += 1
:
:    y = 8
:--
```

With the %cpaste block, you have the freedom to paste as much code as you like before executing it. You might decide to use %cpaste in order to look at the pasted code before executing it. If you accidentally paste the wrong code, you can break out of the %cpaste prompt by pressing Ctrl-C.

### Terminal Keyboard Shortcuts

IPython has many keyboard shortcuts for navigating the prompt (which will be familiar to users of the Emacs text editor or the Unix bash shell) and interacting with the shell’s command history.

| Keyboard Shortcut        | Description          |
|:-------------|:------------------|
| Ctrl-P or up-arrow | Search backward in command history for commands starting with currently entered text |
| Ctrl-N or down-arrow | Search forward in command history for commands starting with currently entered text |
| Ctrl-R | Readline-style reverse history search (partial matching) |
| Ctrl-Shift-V | Paste text from clipboard |
| Ctrl-C | Interrupt currently executing code |
| Ctrl-A | Move cursor to beginning of line |
| Ctrl-E | Move cursor to end of line |
| Ctrl-K | Delete text from cursor until end of line |
| Ctrl-U | Discard all text on current line |
| Ctrl-F | Move cursor forward one character |
| Ctrl-B | Move cursor back one character |
| Ctrl-L | Clear screen |

### About Magic Commands

IPython’s special commands (which are not built into Python itself) are known as “magic” commands. These are designed to facilitate common tasks and enable you to easily control the behavior of the IPython system. A magic command is any command prefixed by the percent symbol %. For example, you can check the execution time of any Python statement, such as a matrix multiplication, using the %timeit magic function (which will be discussed in more detail later) :

```python
In [20]: a = np.random.randn(100, 100)
In [20]: %timeit np.dot(a, a)
```
> 10000 loops, best of 3: 20.9 µs per loop

Magic functions can be used by default without the percent sign, as long as no variable is defined with the same name as the magic function in question. This feature is called automagic and can be enabled or disabled with %automagic. Some frequently used IPython magic commands :

| Command        | Description          |
|:-------------|:------------------|
| %quickref | Display the IPython Quick Reference Card |
| %magic | Display detailed documentation for all of the available magic commands |
| %debug | Enter the interactive debugger at the bottom of the last exception traceback |
| %hist | Print command input (and optionally output) history |
| %pdb | Automatically enter debugger after any exception |
| %paste | Execute preformatted Python code from clipboard |
| %cpaste | Open a special prompt for manually pasting Python code to be executed |
| %reset | Delete all variables/names defined in interactive namespace |
| %page OBJECT | Pretty-print the object and display it through a pager |
| %run script.py | Run a Python script inside IPython |
| %prun statement | Execute statement with cProfile and report the profiler output |
| %time statement | Report the execution time of a single statement |
| %timeit statement | Run a statement multiple times to compute an ensemble average execution time; useful for timing code with very short execution time |
| %who, %who_ls, %whos | Display variables defined in interactive namespace, with varying levels of information/verbosity |
| %xdel variable | Delete a variable and attempt to clear any references to the object in the IPython internals |

## Python Language Basics

### Language Semantics

The Python language design is distinguished by its emphasis on readability, simplicity, and explicitness. Some people go so far as to liken it to “executable pseudocode.” Python uses whitespace (tabs or spaces) to structure code instead of using braces as in many other languages like R, C++, Java, and Perl. Consider a for loop from a sorting algorithm :

```python
for x in array:
    if x < pivot:
        less.append(x)
    else:
        greater.append(x)
```

A colon denotes the start of an indented code block after which all of the code must be indented by the same amount until the end of the block. As you can see by now, Python statements also do not need to be terminated by semicolons. Semicolons can be used, however, to separate multiple statements on a single line :

```pyton
a = 5; b = 6; c = 7
```

An important characteristic of the Python language is the consistency of its object model. Every number, string, data structure, function, class, module, and so on exists in the Python interpreter in its own “box,” which is referred to as a Python object. Each object has an associated type (e.g., string or function) and internal data. Any text preceded by the hash mark (pound sign) # is ignored by the Python interpreter. This is often used to add comments to code.

```python
results = []
for line in file_handle:
    # keep the empty lines for now
    # if len(line) == 0:
    #   continue
    results.append(line.replace('foo', 'bar'))
```

You call functions using parentheses and passing zero or more arguments, optionally assigning the returned value to a variable :

```python
result = f(x, y, z)
g()
```

Almost every object in Python has attached functions, known as methods, that have access to the object’s internal contents. You can call them using the following syntax :

```python
obj.some_method(x, y, z)
```

When assigning a variable (or name) in Python, you are creating a reference to the object on the righthand side of the equals sign. When you pass objects as arguments to a function, new local variables are created referencing the original objects without any copying. If you bind a new object to a variable inside a function, that change will not be reflected in the parent scope. In contrast with many compiled languages, such as Java and C++, object references in Python have no type associated with them. There is no problem with the following :

```python
In [12]: a = 5
In [13]: type(a)
```
> int

Variables are names for objects within a particular namespace; the type information is stored in the object itself. Knowing the type of an object is important, and it’s useful to be able to write functions that can handle many different kinds of input. You can check that an object is an instance of a particular type using the isinstance function :

```python
In [21]: a = 5
In [22]: isinstance(a, int)
```
> True

Often you may not care about the type of an object but rather only whether it has certain methods or behavior. This is sometimes called “duck typing,” after the saying “If it walks like a duck and quacks like a duck, then it’s a duck.” For example, you can verify that an object is iterable if it implemented the iterator protocol. For many objects, this means it has a __iter__ “magic method,” though an alternative and better way to check is to try using the iter function. This function would return True for strings as well as most Python collection types :

```python
In [29]: isiterable('a string')
```
> True

If we wanted to access the variables and functions defined in some_module.py, from another file in the same directory we could do :

```python
import some_module
result = some_module.f(5)
pi = some_module.PI
```

To check if two references refer to the same object, use the is keyword. is not is also perfectly valid if you want to check that two objects are not the same :

```python
In [35]: a = [1, 2, 3]
In [36]: b = a
In [37]: c = list(a)
In [38]: a is b
```
> True

```python
In [39]: a is not c
```
> True

Most objects in Python, such as lists, dicts, NumPy arrays, and most user-defined types (classes), are mutable. This means that the object or values that they contain can be modified :

```python
In [43]: a_list = ['foo', 2, [4, 5]]
In [44]: a_list[2] = (3, 4)
In [45]: a_list
```
> ['foo', 2, (3, 4)]

Others, like strings and tuples, are immutable.

### Scalar Types

Python along with its standard library has a small set of built-in types for handling numerical data, strings, boolean (True or False) values, and dates and time. These “single value” types are sometimes called scalar types.

| Type        | Description          |
|:-------------|:------------------|
| None | The Python “null” value (only one instance of the None object exists) |
| str | String type; holds Unicode (UTF-8 encoded) strings |
| bytes | Raw ASCII bytes (or Unicode encoded as bytes) |
| float | Double-precision (64-bit) floating-point number (note there is no separate double type) |
| bool | A True or False value |
| int | Arbitrary precision signed integer |

The primary Python types for numbers are int and float. An int can store arbitrarily large numbers :

```python
In [48]: ival = 17239871
In [49]: ival ** 6
```
> 26254519291092456596965462913230729701102721

Integer division not resulting in a whole number will always yield a floating-point number :

```python
In [52]: 3 / 2
```
> 1.5

Many people use Python for its powerful and flexible built-in string processing capabilities. You can write string literals using either single quotes ' or double quotes " :

```python
a = 'one way of writing a string'
b = "another way"
```

Python strings are immutable; you cannot modify a string. Strings are a sequence of Unicode characters and therefore can be treated like other sequences, such as lists and tuples. The backslash character \ is an escape character, meaning that it is used to specify special characters like newline \n or Unicode characters. Adding two strings together concatenates them and produces a new string. The two boolean values in Python are written as True and False. Comparisons and other conditional expressions evaluate to either True or False. Boolean values are combined with the and and or keywords :

```python
In [89]: True and True
```
> True

```python
In [90]: False or True
```
> True

None is the Python null value type. If a function does not explicitly return a value, it implicitly returns None :

```python
In [97]: a = None
In [98]: a is None
```
> True

The built-in Python datetime module provides datetime, date, and time types. The datetime type, as you may imagine, combines the information stored in date and time and is the most commonly used :

```python
In [102]: from datetime import datetime, date, time
In [103]: dt = datetime(2011, 10, 29, 20, 30, 21)
In [104]: dt.day
```
> 29

### Control Flow

Python has several built-in keywords for conditional logic, loops, and other standard control flow concepts found in other programming languages. The if statement is one of the most well-known control flow statement types. It checks a condition that, if True, evaluates the code in the block that follows :

```python
if x < 0:
    print('It's negative')
```

An if statement can be optionally followed by one or more elif blocks and a catch-all else block if all of the conditions are False :

```python
if x < 0:
    print('It's negative')
elif x == 0:
    print('Equal to zero')
elif 0 < x < 5:
    print('Positive but smaller than 5')
else:
    print('Positive and larger than or equal to 5')
```

for loops are for iterating over a collection (like a list or tuple) or an iterater. The standard syntax for a for loop is :

```python
for value in collection:
    # do something with value
```

You can advance a for loop to the next iteration, skipping the remainder of the block, using the continue keyword. Consider this code, which sums up integers in a list and skips None values :

```python
sequence = [1, 2, None, 4, None, 5]
total = 0
for value in sequence:
    if value is None:
        continue
    total += value
```

A while loop specifies a condition and a block of code that is to be executed until the condition evaluates to False or the loop is explicitly ended with break :

```python
x = 256
total = 0
while x > 0:
    if total > 500:
        break
    total += x
    x = x // 2
```

pass is the “no-op” statement in Python. It can be used in blocks where no action is to be taken (or as a placeholder for code not yet implemented); it is only required because Python uses whitespace to delimit blocks :

```python
if x < 0:
    print('negative!')
elif x == 0:
    # TODO: put something smart here
    pass
else:
    print('positive!')
```

The range function returns an iterator that yields a sequence of evenly spaced integers :

```python
In [122]: range(10)
```
> range(0, 10)

A ternary expression in Python allows you to combine an if-else block that produces a value into a single line or expression. The syntax for this in Python is :

```python
value = true-expr if condition else false-expr
```

As with if-else blocks, only one of the expressions will be executed. Thus, the “if ” and “else” sides of the ternary expression could contain costly computations, but only the true branch is ever evaluated.
