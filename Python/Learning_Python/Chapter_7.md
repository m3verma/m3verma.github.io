---
layout: default
---

# String Fundamentals

### String Basics

From a functional perspective, strings can be used to represent just about anything that can be encoded as text or bytes. In the text department, this includes symbols and words (e.g., your name), contents of text files loaded into memory, Internet addresses, Python source code, and so on. Python strings are categorized as immutable sequences, meaning that the characters they contain have a left-to-right positional order and that they cannot be changed in place. Common string literals and operations :

| Operation        | Interpretation          |
|:-------------|:------------------|
| S = ''           | Empty string |
| S = "spam's" | Double quotes, same as single  |
| S = 's\np\ta\x00m'           | Escape sequences  |
| S = """...multiline..."""           | Triple-quoted block strings |
| S = r'\temp\spam'           | Raw strings (no escapes) |
| B = b'sp\xc4m' | Byte strings in 2.6, 2.7, and 3.X   |
| U = u'sp\u00c4m'           | Unicode strings in 2.X and 3.3+    |
| S1 + S2           | Concatenate, repeat |
| ```S[i]``` | Index   |
| ```S[i:j]```           | Slice    |
| len(S)          | Length |




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
