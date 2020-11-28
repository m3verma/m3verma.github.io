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
| "a %s parrot" % kind | String formatting expression   |
| "a {0} parrot".format(kind)           | String formatting method in 2.6, 2.7, and 3.X   |
| S.find('pa')           | String methods (see ahead for all 43): search, |
| S.rstrip() | Remove whitespace,   |
| S.replace('pa', 'xx')           | Replacement    |
| S.split(',')          | Split on delimiter |
| S.isdigit()          | Content test |
| S.lower() | Case conversion   |
| S.endswith('spam')           | End Test   |
| 'spam'.join(strlist)           | Delimiter join |
| S.encode('latin-1') | Unicode encoding   |
| B.decode('utf8')           | Unicode decoding    |
| for x in S: print(x)        | Iteration |

### String Literals

By and large, strings are fairly easy to use in Python. Perhaps the most complicated thing about them is that there are so many ways to write them in your code:

1. Single quotes: 'spa"m'
2. Double quotes: "spa'm"
3. Triple quotes: '''... spam ...''', """... spam ..."""
4. Escape sequences: "s\tp\na\0m"
5. Raw strings: r"C:\new\test.spm"
6. Bytes literals in 3.X and 2.6+ : b'sp\x01am'
7. Unicode literals in 2.X and 3.3+ : u'eggs\u0020spam'

Around Python strings, single- and double-quote characters are interchangeable. That is, string literals can be written enclosed in either two single or two double quotes—the two forms work the same and return the same type of object. The reason for supporting both is that it allows you to embed a quote character of the other variety inside a string without escaping it with a backslash.

```python
>>> 'shrubbery', "shrubbery"
```
> ('shrubbery', 'shrubbery')

Python automatically concatenates adjacent string literals in any expression, although it is almost as simple to add a + operator between them to invoke concatenation explicitly.

```python
>>> title = "Meaning " 'of' " Life"        # Implicit concatenation
>>> title
```
> 'Meaning of Life'

### Escape Sequences

Backslashes are used to introduce special character codings known as escape sequences. Escape sequences let us embed characters in strings that cannot easily be typed on a keyboard. The character \, and one or more characters following it in the string literal, are replaced with a single character in the resulting string object, which has the binary value specified by the escape sequence. For example, here is a five-character string that embeds a newline and a tab :

```python
>>> s = 'a\nb\tc'
```
> 'a\nb\tc'

```python
>>> print(s)
```
> a
> b       c

Python recognizes a full set of escape code sequences :

| Escape        | Meaning          |
|:-------------|:------------------|
| \newline | Ignored (continuation line) |
| \\ | Backslash (stores one \) |
| `\'`  | Single quote (stores ') |
| `\"` | Double quote (stores ") |
| \a | Bell |
| \b | Backspace |
| \f | Formfeed |
| \n | Newline (linefeed) |
| \r | Carriage return |
| \t | Horizontal tab |
| \v | Vertical tab |
| \xhh | Character with hex value hh (exactly 2 digits) |
| \ooo | Character with octal value ooo (up to 3 digits) |
| \0 | Null: binary 0 character (doesn’t end string) |
| \N{ id } | Unicode database ID |
| \uhhhh | Unicode character with 16-bit hex value |
| \Uhhhhhhhh | Unicode character with 32-bit hex valuea |
| \other | Not an escape (keeps both \ and other) |

Some escape sequences allow you to embed absolute binary values into the characters of a string. For instance, here’s a five-character string that embeds two characters with binary zero values :

```python
>>> s = 'a\0b\0c'
>>> s
```
> 'a\x00b\x00c'

```python
>>> len(s)
```
> 5

If the letter r (uppercase or lowercase) appears just before the opening quote of a string, it turns off the escape mechanism. The result is that Python retains your backslashes literally, exactly as you type them. Alternatively, because two backslashes are really an escape sequence for one backslash, you can keep your backslashes by simply doubling them up.

```python
>>> myfile = open(r'C:\new\text.dat', 'w')
>>> myfile = open('C:\\new\\text.dat', 'w')
```

### Triple Quotes Code

Python also has a triple-quoted string literal format, sometimes called a block string, that is a syntactic convenience for coding multiline text data. This form begins with three quotes (of either the single or double variety), is followed by any number of lines of text, and is closed with the same triple-quote sequence that opened it. In fact, triple-quoted strings will retain all the enclosed text, including any to the right of your code that you might intend as comments. Triple-quoted strings are useful anytime you need multiline text in your program; for example, to embed multiline error messages or HTML, XML, or JSON code in your Python source code files.

```python
>>> mantra = """Always look
...   on the bright
... side of life."""
>>>
>>> mantra
```
> 'Always look\n  on the bright\nside of life.'

### Strings in Action

You can concatenate strings using the + operator and repeat them using the * operator:

```python
>>> len('abc')            # Length: number of items
```
> 3

```python
>>> 'abc' + 'def'         # Concatenation: a new string
```
> 'abcdef'

```python
>>> 'Ni!' * 4             # Repetition: like "Ni!" + "Ni!" + ...
```
> 'Ni!Ni!Ni!Ni!'

The len built-in function here returns the length of a string. Notice that operator overloading is at work here already: we’re using the same + and * operators that perform addition and multiplication when using numbers. Python does the correct operation because it knows the types of the objects being added and multiplied. But be careful: the rules aren’t quite as liberal as you might expect. For instance, Python doesn’t allow you to mix numbers and strings in + expressions: 'abc'+9 raises an error instead of automatically converting 9 to a string. You can also iterate over strings in loops using for statements, which repeat actions, and test membership for both characters and substrings with the in expression operator, which is essentially a search.

```python
>>> "k" in myjob                        # Found
```
> True

```python
>>> "z" in myjob                        # Not found
```
> False

### Indexing and Slicing




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
