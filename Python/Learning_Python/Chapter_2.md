---
layout: default
---

# How Python Runs Program

### Interpreter

An interpreter is a kind of program that executes other programs.

### Download Path

You can Download here - [Python](www.python.org)
<br><br>
*   In its simplest form, a python program is just a text file containing python statements.
<br><br>
*   Python program files are given names that end in **.py**

### Virtual Machine

When we instruct python to run our scripts, there are a few steps that python carries out before our code actually starts crunching away. Specifically, it’s first compiled to something called “byte code” and then routed to something called “virtual machine”.
<br><br>
*   Python first compiles source code into format known as **byte code**. Python store the byte code of our program in files that end in **.pyc** extension.
<br><br>
*   Python saves its **.pyc** byte code files in a subdirectory named **__pycache__** located in the directory where our source code files reside, and in those files whose names identify the python version that created them.

### Python Recompiles When
1. **Source Changes** - Python automatically checks the last modified timestamps of source and byte code files to know when it must recompile.
2. **Python Versions** - Imports also check to see if the file must be recompiled because it was created by a different python version.

### PVM
Once program has been compiled to byte code, it is shipped off for execution to something generally known as **Python Virtual Machine**. PVM is just a big code loop that iterates through our byte code instructions, one by one, to carry out their operations. The PVM is the **runtime engine** of Python. It’s always present as part of the python system and it’s the component that truly runs our scripts.
<br><br>
*   Byte code is python specific representation that’s why Python code may not run as fast as C or C++ code.
<br><br>
*   In python’s execution model, there is no distinction between the development and execution environments. Due to this python code can be changed on the fly, users can modify the python parts of the system onsite without needing to have or compile the entire system’s code.

### Python Implementation alternatives -
1. **CPython** - The original, and standard, implementation of Python is usually called CPython when you want to contrast it with the other options (and just plain “Python” otherwise). This name comes from the fact that it is coded in portable ANSI C language code.
2. **Jython** - The Jython system (originally known as JPython) is an alternative implementation of the Python language, targeted for integration with the Java programming language. Jython consists of Java classes that compile Python source code to Java byte code and then route the resulting byte code to the Java Virtual Machine (JVM)
3. **IronPython** - IronPython is designed to allow Python programs to integrate with applications coded to work with Microsoft’s .NET Framework for Windows, as well as the Mono open source equivalent for Linux.
4. **Stackless** - Stackless Python system is an enhanced version and reimplementation of the standard CPython language oriented toward concurrency.
5. **PyPy** - The PyPy system is another standard CPython reimplementation, focused on performance. It provides a fast Python implementation with a JIT (just-in-time) compiler, provides tools for a “sandbox” model that can run untrusted code in a secure environment, and by default includes support for the prior section’s Stackless Python systems and its microthreads to support massive concurrency.

### Why PyPy is fast?
A JIT is really just an extension to the PVM that translates portions of your byte code all the way to binary machine code for faster execution. It does this as your program is running, not in a prerun compile step, and is able to created type-specific machine code for the dynamic Python language by keeping track of the data types of the objects your program processes. By replacing portions of your byte code this way, your program runs faster and faster as it is executing. In addition, some Python programs may also take up less memory under PyPy.

### Frozen Binaries
With the help of third-party tools that you can fetch off the Web, it is possible to turn your Python programs into true executables, known as frozen binaries in the Python world. These programs can be run without requiring a Python installation. Frozen binaries bundle together the byte code of your program files, along with the PVM (interpreter) and any Python support files your program needs, into a single package
