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
>>> X     #Mistake Variables
```
> Traceback (most recent call last):
>  File "<stdin>", line 1, in <module>
> NameError: name 'X' is not defined


* * *

# Test Your Knowledge

### Q2 - What is source code?

```
Source code is the statements you write for your program—it consists of text in text files that normally
end with a .py extension.
```
