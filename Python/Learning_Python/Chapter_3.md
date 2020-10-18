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


* * *

# Test Your Knowledge

### Q2 - What is source code?

```
Source code is the statements you write for your program—it consists of text in text files that normally
end with a .py extension.
```
