---
layout: default
---

# How You Run Programs

### Starting an Interactive Session

Simplest way to run Python programs is to type them at Python’s interactive command line, sometimes called the interactive prompt. The most platform-neutral way to start an interactive interpreter session is usually just to type python at your operating system’s prompt, without any arguments.

```python
% python
Python 3.3.0 (v3.3.0:bd8afb90ebf2, Sep 29 2012, 10:57:17) [MSC v.1600 64 bit ...
Type "help", "copyright", "credits" or "license" for more information.
>>> ^Z
```
Anytime you see the ">>>" prompt, you’re in an interactive Python interpreter session. You can type any Python statement or expression here and run it immediately.

### System PATH

Starting with Python 3.3, the Windows installer has an option to automatically add Python 3.3’s directory to your system PATH, if enabled in the installer’s windows.

### Running Code

```python
% python
>>> print('Hello world!')
```
> Hello world!

```python
% python
>>> print(2 ** 8)
```
> 256

When coding interactively like this, you can type as many Python commands as you like; each is run immediately after it’s entered. Moreover, because the interactive session automatically prints the results of expressions you type, you don’t usually need to say “print” explicitly at this prompt.

```python
>>> lumberjack = 'okay'
>>> lumberjack
```
> 'okay'

```python
>>> 2 ** 8
```
> 256

### Why the Interactive Prompt

The interactive prompt runs code and echoes results as you go, but it doesn’t save your code in a file. Because code is executed immediately, the interactive prompt is a perfect place to experiment with the language and will be used often in this website to demonstrate smaller examples. The immediate feedback you receive at the interactive prompt is often the quickest way to deduce what a piece of code does. 

```python
>>> 'Spam!' * 8
```
> 'Spam!Spam!Spam!Spam!Spam!Spam!Spam!Spam!'

Here, it’s clear that it does string repetition: in Python * means multiply for numbers, but repeat for strings, it’s like concatenating a string to itself repeatedly

<br>
*   In Python, using a variable before it has been assigned a value is always an error otherwise, if names were filled in with defaults, some errors might go undetected. You don’t declare variables, but they must be assigned before you can fetch their values.

```python
>>> X     # Mistake Variables
```
> Traceback (most recent call last):
>  File "<stdin>", line 1, in <module>
> NameError: name 'X' is not defined

### Testing 

The interactive interpreter is also an ideal place to test code you’ve written in files. You can import your module files interactively and run tests on the tools they define by typing calls at the interactive prompt on the fly.

### Usage Notes : Interactive Prompt

1.	Type Python commands only. First of all, remember that you can only type Python code at Python’s prompt, not system commands.
2.	Print statements are required only in files. Because the interactive interpreter automatically prints the results of expressions, you do not need to type complete print statements interactively.
3.	Don’t indent at the interactive prompt (yet). When typing Python programs, either interactively or into a text file, be sure to start all your unnested statements in column 1 (that is, all the way to the left). If you don’t, Python may print a “SyntaxError” message, because blank space to the left of your code is taken to be indentation that groups nested statements
4.	Watch out for prompt changes for compound statements. You should know that when typing lines 2 and beyond of a compound statement interactively, the prompt may change. In the simple shell window interface, the interactive prompt changes to "..." instead of ">>>" for lines 2 and beyond
5.	Terminate compound statements at the interactive prompt with a blank line. At the interactive prompt, inserting a blank line (by hitting the Enter key at the start of a line) is necessary to tell interactive Python that you’re done typing the multiline statement
6.	The interactive prompt runs one statement at a time. At the interactive prompt, you must run one statement to completion before typing another.

### Multiline statement 

Be sure to terminate multiline compound statements like for loops and if tests at the interactive prompt with a blank line. In other words, you must press the Enter key twice. Also bear in mind that the interactive prompt runs just one statement at a time: you must press Enter twice to run a loop or other multiline statement before you can type the next statement

```python
>>> for x in 'spam':
...     print(x)              # Press Enter twice here to make this loop run
...
```

### Files

Although the interactive prompt is great for experimenting and testing, it has one big disadvantage: programs you type there go away as soon as the Python interpreter executes them. Because the code you type interactively is never stored in a file, you can’t run it again without retyping it from scratch. To save programs permanently, you need to write your code in files, which are usually known as modules. Python executes all the code in a module file from top to bottom each time you run the file.

```python
# A first Python script
import sys                  # Load a library module
print(sys.platform)
print(2 ** 100)             # Raise 2 to a power
x = 'Spam!'
print(x * 8)                # String repetition
```

<br>
*   The **sys.platform** here is just a string that identifies the kind of computer you’re working on; it lives in a standard Python module called sys, which you must import to load.

### Running Files

Once you’ve saved this text file, you can ask Python to run it by listing its full filename as the first argument to a python command like the following typed at the system shell prompt.

```python
% python script1.py
```

> win32

> 1267650600228229401496703205376

> Spam!Spam!Spam!Spam!Spam!Spam!Spam!Spam!

### Command line usage of python

Because this scheme uses shell command lines to start Python programs, all the usual shell syntax applies. For instance, you can route the printed output of a Python script to a file to save it for later use or inspection by using special shell syntax:

```python
% python script1.py > saveit.txt
```

In this case, the three output lines shown in the prior run are stored in the file saveit.txt instead of being printed

<br>
*   On all recent versions of Windows, you can also type just the name of your script, and omit the name of Python itself. Because newer Windows systems use the Windows Registry (a.k.a. filename associations) to find a program with which to run a file, you don’t need to name “python” or “py” on the command line explicitly to run a .py file.

### Usage Notes : Command Lines and Files

1.	Beware of automatic extensions on Windows and IDLE
2.	Use file extensions and directory paths at system prompts, but not for imports
3.	Use print statements in files

### Script icons 

Icon clicks allow you to run the file without any typing at all. To do so, you have to find this file’s icon on your computer. By default, Python generates a pop-up black DOS console window (Command Prompt) to serve as a clicked file’s input and output.

### Example script 

```python
# A first Python script
import sys                  # Load a library module
print(sys.platform)
print(2 ** 100)             # Raise 2 to a power
x = 'Spam!'
print(x * 8)                # String repetition
input()                     # <== ADDED
```

For instance, input:
1. Optionally accepts a string that will be printed as a prompt (e.g., input('Press Enter to exit'))
2. Returns to your script a line of text read as a string (e.g., nextinput = input())
3. Supports input stream redirections at the system shell level (e.g., python spam.py < input.txt), just as the print statement does for output.

<br>
*   Files whose names end in a .pyw extension will display only windows constructed by your script, not the default console window. .pyw files are simply .py source files that have this special operational behavior on Windows

### Icon click limitations 

If your script generates an error, the error message text is written to the pop-up console window—which then immediately disappears! Worse, adding an input call to your file will not help this time because your script will likely abort long before it reaches this call. In other words, you won’t be able to tell what went wrong.

### Import and reload basics

1.	In simple terms, every file of Python source code whose name ends in a .py extension is a module. No special code or syntax is required to make a file a module: any such file will do.
2.	Larger programs usually take the form of multiple module files, which import tools from other module files.
3.	Import - This works, but only once per session (really, process—a program run) by default. After the first import, later imports do nothing, even if you change and save the module’s source file again in another window
4.	If you really want to force Python to run the file again in the same session without stopping and restarting the session, you need to instead call the reload function available in the imp standard library module. 
5.	The reload function expects the name of an already loaded module object, so you have to have successfully imported a module once before you reload it
6.	Moreover, reloads aren’t transitive—reloading a module reloads that module only, not any modules it may import—so you sometimes have to reload multiple files.

```python
>>> from imp import reload
>>> reload(script1)
```

### Modules – 

1.	The basic idea is straight forward, though: a module is mostly just a package of variable names, known as a namespace, and the names within that package are called attributes. An attribute is simply a variable name that is attached to a specific object
2.	Technically, from copies a module’s attributes, such that they become simple variables in the recipient—thus, you can simply refer to the imported string this time as title (a variable) instead of myfile.title
3.	Once you start coding modules with multiple names like this, the built-in dir function starts to come in handy—you can use it to fetch a list of all the names available inside a module.
4.	When the dir function is called with the name of an imported module in parentheses like this, it returns all the attributes inside that module. Some of the names it returns are names you get “for free”: names with leading and trailing double underscores (__X__) are built-in names that are always predefined by Python and have special meaning to the interpreter.

### Using exec to run modules 

There are more ways to run code stored in module files than have yet been presented here. For instance, the **exec(open('module.py').read())** built-in function call is another way to launch files from the interactive prompt without having to import and later reload. Each such exec runs the current version of the code read from a file, without requiring later reloads. The exec call has an effect similar to an import, but it doesn’t actually import the module —by default, each time you call exec this way it runs the file’s code anew, as though you had pasted it in at the place where exec is called.

```python
% python
>>> exec(open('script1.py').read())
```

> win32

> 65536

> Spam!Spam!Spam!Spam!Spam!Spam!Spam!Spam!

### IDLE

If you’re looking for something a bit more visual, IDLE provides a graphical user interface for doing Python development, and it’s a standard and free part of the Python system. IDLE is usually referred to as an integrated development environment (IDE). IDLE is a desktop GUI that lets you edit, run, browse, and debug Python programs, all from a single interface. It is installed automatically with standard Python on Windows. Technically, IDLE is a Python program that uses the standard library’s tkinter GUI toolkit (named Tkinter in Python 2.X) to build its windows.

```python
%python -m idlelib.idle           # Run idle.py in a package on module path
```


* * *

# Test Your Knowledge

### Q2 - What is source code?

```
Source code is the statements you write for your program—it consists of text in text files that normally
end with a .py extension.
```
