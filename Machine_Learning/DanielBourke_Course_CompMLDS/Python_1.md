---
layout: default
---

# Python

Programming is simply a way for us to give instructions to computers, we give it an instruction manual and the computer follows. Computers don't understand English or any other human language for that matter. They speak in ones and zeros. All electronics speak that language of on or off, zero or one. But writing ones and zeros would be gibberish for us, it's hard for us to communicate like that, right? So humans have devolved programming languages that are in between human language and machine language that is zeros and some programming languages are lower level than others. You have languages like Python and JavaScript that are really, really close to English.

We take our code that we write in what we call a source code written in a programming languages, and we give that to a translator that can understand that language but also understands machine language. And this translator just translates these files for us. Python usually uses an interpreter. And an interpreter, just like a translator, goes line by line through our code and executes our code on our machine. Compilers are a little bit different. They take your code all at once with the entire file all at once and then translates that to machine. So again, interpretor goes line by line and each line executes an instruction. A compiler takes the entire file and turns it into machine code.

To download python - [Python Website](https://python.org)

First, we're going to have our terminal or command line where we can run Python from right in what we call a ripple. And then we're also going to use code editors like sublime text and Visual Studio Code. We're also going to use IDE or integrated developer environments like PyCharm and Spider. And we're also going to use Jupyter notebooks. Instead of installing all these we can go to website called [Replit](https://replit.com/). Create an account here and its going to be worth your time. Once we log in and create an account we can directly start writing python code without downloading anything.

## First Python Program

Let's create a very, very simple function and probably the first thing any programmer learns to do to print our own name. Go to replit and write the below code in right hand side :

```python
>>> print("Tony Stark")               # Print's your name
```
> Tony Stark

In left you can see that we have a "main.py" file. Any file which has .py extension is a python file. If you see that we are using python version 3.8.2 (Python version is continously going to upgrade). This is the CPython VM essentially running this code and telling this machine, "Hey, I want you to print this". Let's make it more dynamic by asking user its name and saving it to a variable :

```python
>>> name = input("What is your name ? ")
>>> print("Hello! " + name)
```
> What is your name ? Tony Stark <br>
> Hello! Tony Stark

## Python 2 vs Python 3

Python was created in 1991 by Guido Van Rossum and was actually named from Monty Python. Now, all programming languages are constantly being updated. In 2008 Guido decided to change some things about python so they created Python 3. Python 3 introduced some breaking changes. You usually want to keep up with the latest version as it has the community support, most up to the date. Some breaking changes :

1. Division Operator - (7/5) in python 2.x will give 1 but in python 3.x will give 1.4
2. Print Function - In python 2.x - print 'Hello' while in python 3.x - print('hello')
3. Unicode - In Python 2, an implicit str type is ASCII. But in Python 3.x implicit str type is Unicode. 
4. xrange - xrange() of Python 2.x doesn’t exist in Python 3.x.
5. Error Handling - There is a small change in error handling. In python 3.x, ‘as’ keyword is required. 

## Python Data types

The basic data types available in Python are similar to what you’re already used in other programming languages :

1. Integers
2. Floats (reals)
3. Booleans (logicals)
4. Strings
5. Lists

### Integers

Integers are one of the types that can be used to represent numbers in Python. These can be both positive and negative values and may be arbitrarily large. The following are a few examples of these :

```python
num = 73 # Positive number
print(num) 
neg_num = -15 # Negative number
print(neg_num)
largeNumber = 12345678901234567890 
print(largeNumber) 
```
> 73<br>
> -15<br>
> 12345678901234567890

### Floats

Floats are another form of representing numbers in Python and exist as either positive or negative decimal values. Below are some examples of floats :

```python
pie_value = 3.1416
print(pie_value)
std_form = 6.02200000000000000000000
print(std_form) 
```
> 3.1416<br>
> 6.022

### Boolean

Boolean is a built-in logical data type that is mainly used for checking whether the logic of an expression or comparison is true or not. Boolean variables can only have two possible values : True or False. A few examples of these might be :

```python
print(True)
print(False)
true_var = True
false_var = False
print(true_var)
print(false_var)
```
> True<br>
> False<br>
> True<br>
> False

### Strings

The string data type is used for representing characters. In Python, there are no major restrictions for using strings. You can use either single or double quotes to represent them. You can even insert single quotes inside double-quoted strings, and vice versa.

```python
print("Hello World")
print('Goodbye')
print("##@@")
print("Tom said, 'it has been raining a lot these days'.") # single quotes inside doubly-quoted strings
print("") # A string containing no characters is an "empty string"
```
> Hello World<br>
> Goodbye<br>
> ##@@<br>
> Tom said, 'it has been raining a lot these days'.

### Lists

Lists are similar to collections, as in you can store multiple items of different data types inside of them. These items are stored with a fixed and defined order and are easily changeable.

```python
my_list_emp = [] # empty list
print(my_list_emp)
my_list_int = [1, 2, 3, 4, 5] # list of integers
print(my_list_int)
my_list = [1, 2, "Buckle my shoe"] # list of mixed data types
print(my_list)
```
> []
> [1, 2, 3, 4, 5]
> [1, 2, 'Buckle my shoe']