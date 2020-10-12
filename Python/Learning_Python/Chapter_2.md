---
layout: default
---

# How Python Runs Program

### Interpreter

An interpreter is a kind of program that executes other programs.

### Download Path

You can Download here - [Python](www.python.org)

*   In its simplest form, a python program is just a text file containing python statements.
<br>
*   Python program files are given names that end in **.py**

### Virtual Machine

When we instruct python to run our scripts, there are a few steps that python carries out before our code actually starts crunching away. Specifically, it’s first compiled to something called “byte code” and then routed to something called “virtual machine”.

*   Python first compiles source code into format known as **byte code**. Python store the byte code of our program in files that end in **.pyc** extension.
<br>
*   Python saves its **.pyc** byte code files in a subdirectory named **__pycache__** located in the directory where our source code files reside, and in those files whose names identify the python version that created them.

### Python Recompiles When
1. **Source Changes** - Python automatically checks the last modified timestamps of source and byte code files to know when it must recompile.
2. **Python Versions** - Imports also check to see if the file must be recompiled because it was created by a different python version.
