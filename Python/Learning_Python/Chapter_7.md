---
layout: default
---

# String Fundamentals

### String Basics

When we type a = 3 in an interactive session or program file, for instance, how does Python know that a should stand for an integer? For that matter, how does Python know what a is at all? In Python, types are determined automatically at runtime, not in response to declarations in your code. This means that you never declare variables ahead of time.




* * *

# Test Your Knowledge

### Q1 -  Consider the following three statements. Do they change the value printed for A?

```python
A = "spam"
B = A
B = "shrubbery"
```
```
No: A still prints as "spam". When B is assigned to the string "shrubbery", all that happens is 
that the variable B is reset to point to the new string object. A and B initially share 
(i.e., reference/point to) the same single string object "spam", but two names are never linked 
together in Python. Thus, setting B to a different object has no effect on A. The same would be 
true if the last statement here were B = B + 'shrubbery', by the way—the concatenation would make 
a new object for its result, which would then be assigned to B only. We can never overwrite a 
string (or number, or tuple) in place, because strings are immutable.
```
