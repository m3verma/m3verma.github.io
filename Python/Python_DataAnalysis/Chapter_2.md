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
