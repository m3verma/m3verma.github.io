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

In Python, characters in a string are fetched by indexing—providing the numeric offset of the desired component in square brackets after the string. You get back the one-character string at the specified position. As in the C language, Python offsets start at 0 and end at one less than the length of the string. Unlike C, however, Python also lets you fetch items from sequences such as strings using negative offsets. Technically, a negative offset is added to the length of a string to derive a positive offset. You can also think of negative offsets as counting backward from the end.

```python
>>> S = 'spam'
>>> S[0], S[−2]                         # Indexing from front or end
```
> ('s', 'a')

```python
>>> S[1:3], S[1:], S[:−1]               # Slicing: extract a section
```
> ('pa', 'pam', 'spa')

The last line in the preceding example demonstrates slicing, a generalized form of indexing that returns an entire section, not a single item. The basics of slicing are straightforward. When you index a sequence object such as a string on a pair of offsets separated by a colon, Python returns a new object containing the contiguous section identified by the offset pair. The left offset is taken to be the lower bound (inclusive), and the right is the upper bound (noninclusive).

Indexing `(S[i])` fetches components at offsets:
1. The first item is at offset 0.
2. Negative indexes mean to count backward from the end or right.
3. `S[0]` fetches the first item.
4. `S[−2]` fetches the second item from the end (like `S[len(S)−2]`).

Slicing `(S[i:j])` extracts contiguous sections of sequences:
1. The upper bound is noninclusive.
2. Slice boundaries default to 0 and the sequence length, if omitted.
3. `S[1:3]` fetches items at offsets 1 up to but not including 3.
4. `S[1:]` fetches items at offset 1 through the end (the sequence length).
5. `S[:3]` fetches items at offset 0 up to but not including 3.
6. `S[:−1]` fetches items at offset 0 up to but not including the last item.
7. `S[:]` fetches items at offsets 0 through the end—making a top-level copy of S.

In Python 2.3 and later, slice expressions have support for an optional third index, used as a step. The full-blown form of a slice is now `X[I:J:K]`, which means “extract all the items in X, from offset I through J−1, by K.” The third limit, K, defaults to +1, which is why normally all items in a slice are extracted from left to right. For instance, `X[1:10:2]` will fetch every other item in X from offsets 1–9; that is, it will collect the items at offsets 1, 3, 5, 7, and 9.

```python
>>> S = 'abcdefghijklmnop'
>>> S[1:10:2]                          # Skipping items
```
> 'bdfhj'

```python
>>> S[::2]
```
> 'acegikmo'

```python
>>> S = 'hello'
>>> S[::−1]                            # Reversing items
```
> 'olleh'

### String Conversion Tools

You cannot add a number and a string together in Python, even if the string looks like a number.

```python
>>> "42" + 1
```
> TypeError: Can't convert 'int' object to str implicitly

What to do, then, if your script obtains a number as a text string from a file or user interface? The trick is that you need to employ conversion tools before you can treat a string like a number, or vice versa. The int function converts a string to a number, and the str function converts a number to its string representation.

```python
>>> int("42"), str(42)          # Convert from/to string
```
> (42, '42')

On the subject of conversions, it is also possible to convert a single character to its underlying integer code (e.g., its ASCII byte value) by passing it to the built-in ord function—this returns the actual binary value used to represent the corresponding character in memory. The chr function performs the inverse operation, taking an integer code and converting it to the corresponding character :

```python
>>> ord('s')
```
> 115

```python
>>> chr(115)
```
> 's'

### Changing Strings

Remember the term “immutable sequence”? As we’ve seen, the immutable part means that you cannot change a string in place—for instance, by assigning to an index:

```python
>>> S = 'spam'
>>> S[0] = 'x'                 # Raises an error!
```
> TypeError: 'str' object does not support item assignment

How to modify text information in Python, then? To change a string, you generally need to build and assign a new string using tools such as concatenation and slicing, and then, if desired, assign the result back to the string’s original name :

```python
>>> S = S + 'SPAM!'            # To change a string, make a new one
>>> S
```
> 'spamSPAM!'

```python
>>> S = S[:4] + 'Burger' + S[−1]
>>> S
```
> 'spamBurger!'

### String Methods

In addition to expression operators, strings provide a set of methods that implement more sophisticated text-processing tasks. In Python, expressions and built-in functions may work across a range of types, but methods are generally specific to object types—string methods, for example, work only on string objects. String methods in Python :

| Functions        | Functions          |
|:-------------|:------------------|
| S.capitalize() | `S.ljust(width [, fill])` |
| S.casefold() | S.lower() |
| `S.center(width [, fill])` | `S.lstrip([chars])` |
| `S.count(sub [, start [, end]])` | `S.maketrans(x[, y[, z]])` |
| `S.encode([encoding [,errors]])` | S.partition(sep) |
| `S.endswith(suffix [, start [, end]])` | `S.replace(old, new [, count])` |
| `S.expandtabs([tabsize])` | `S.rfind(sub [,start [,end]])` |
| `S.find(sub [, start [, end]])` | `S.rindex(sub [, start [, end]])` |
| `S.index(sub [, start [, end]])` | S.rpartition(sep) |
| S.isalnum() | `S.rsplit([sep[, maxsplit]])` |
| S.isalpha() | `S.rstrip([chars])` |
| S.isdecimal() | `S.split([sep [,maxsplit]])` |
| S.isdigit() | `S.splitlines([keepends])` |
| S.isidentifier() | `S.startswith(prefix [, start [, end]])` |
| S.islower() | `S.strip([chars])` |
| S.isnumeric() | S.swapcase() |
| S.isprintable() | S.title() |
| S.isspace() | S.translate(map) |
| S.istitle() | S.upper() |
| S.isupper() | S.zfill(width) |
| S.join(iterable) | `S.rjust(width [, fill])` |

The replace method is more general than this code implies. It takes as arguments the original substring (of any length) and the string (of any length) to replace it with, and performs a global search and replace:

```python
>>> 'aa$bb$cc$dd'.replace('$', 'SPAM')
```
> 'aaSPAMbbSPAMccSPAMdd'

```python
>>> 'xxxxSPAMxxxxSPAMxxxx'.replace('SPAM', 'EGGS', 1)      # Replace one
```
> 'xxxxEGGSxxxxSPAMxxxx'

Notice that replace returns a new string object each time. Because strings are immutable, methods never really change the subject strings in place, even if they are called “replace”!

```python
>>> S = 'spammy'
>>> L = list(S)
>>> L
```
> 's', 'p', 'a', 'm', 'm', 'y'

The built-in list function (an object construction call) builds a new list out of the items in any sequence—in this case, “exploding” the characters of a string into a list. Once the string is in this form, you can make multiple changes to it without generating a new copy for each change :

```python
>>> L[3] = 'x'                        # Works for lists, not strings
>>> L[4] = 'x'
>>> L
```
> 's', 'p', 'a', 'x', 'x', 'y'

If, after your changes, you need to convert back to a string (e.g., to write to a file), use the string join method to “implode” the list back into a string :

```python
>>> S = ''.join(L)
>>> S
```
> 'spaxxy'

Another common role for string methods is as a simple form of text parsing—that is, analyzing structure and extracting substrings. The string split method chops up a string into a list of substrings, around a delimiter string. We didn’t pass a delimiter in the prior example, so it defaults to whitespace.

```python
>>> line = 'aaa bbb  ccc'
>>> cols = line.split()
>>> cols
```
> 'aaa', 'bbb', 'ccc'

Delimiters can be longer than a single character, too :

```python
>>> line = "i'mSPAMaSPAMlumberjack"
>>> line.split("SPAM")
```
> "i'm", 'a', 'lumberjack'

Other string methods have more focused roles—for example, to strip off whitespace at the end of a line of text, perform case conversions, test content, and test for a substring at the end or front :

```python
>>> line = "The knights who say Ni!\n"
>>> line.rstrip()
```
> 'The knights who say Ni!'

```python
>>> line.upper()
```
> 'THE KNIGHTS WHO SAY NI!\n'

```python
>>> line.isalpha()
```
> False

```python
>>> line.endswith('Ni!\n')
```
> True

```python
>>> line.startswith('The')
```
> True

### String Formatting Expressions

String formatting allows us to perform multiple type-specific substitutions on a string in a single step. It’s never strictly required, but it can be convenient, especially when formatting text to be displayed to a program’s users. String formatting is available in two flavors in Python today :

1. String formatting expressions: '...%s...' % (values)
2. String formatting method calls: '...{}...'.format(values)

When applied to strings, the % operator provides a simple way to format values as strings according to a format definition. To format strings:

1. On the left of the % operator, provide a format string containing one or more embedded conversion targets, each of which starts with a % (e.g., %d).
2. On the right of the % operator, provide the object (or objects, embedded in a tuple) that you want Python to insert into the format string on the left in place of the conversion target (or targets).

```python
>>> 'That is %d %s bird!' % (1, 'dead')             # Format expression
```
> That is 1 dead bird!

```python
>>> '%s -- %s -- %s' % (42, 3.14159, [1, 2, 3])     # All types match a %s target
```
> '42 -- 3.14159 -- 1, 2, 3'

For more advanced type-specific formatting, you can use any of the conversion type codes :

| Code        | Meaning          |
|:-------------|:------------------|
| s | String (or any object’s str(X) string) |
| r | Same as s, but uses repr, not str |
| c | Character (int or str) |
| d | Decimal (base-10 integer) |
| i | Integer |
| u | Same as d (obsolete: no longer unsigned) |
| o | Octal integer (base 8) |
| x | Hex integer (base 16) |
| X | Same as x, but with uppercase letters |
| e | Floating point with exponent, lowercase |
| E | Same as e, but uses uppercase letters |
| f | Floating-point decimal |
| F | Same as f, but uses uppercase letters |
| g | Floating-point e or f |
| G | Floating-point E or F |
| % | Literal % (coded as %%) |

This one formats integers by default, and then in a six-character field with left justification and zero padding:

```python
>>> x = 1234
>>> res = 'integers: ...%d...%−6d...%06d' % (x, x, x)
>>> res
```
> 'integers: ...1234...1234  ...001234'

The following interaction demonstrates—%E is the same as %e but the exponent is uppercase :

```python
>>> x = 1.23456789
>>> '%e | %f | %g' % (x, x, x)
```
> '1.234568e+00 | 1.234568 | 1.23457'

```python
>>> '%E' % x
```
> '1.234568E+00'

As a more advanced extension, string formatting also allows conversion targets on the left to refer to the keys in a dictionary coded on the right and fetch the corresponding values.

```python
>>> '%(qty)d more %(food)s' % {'qty': 1, 'food': 'spam'}
```      
> '1 more spam'

```python
>>> reply = """
Greetings...
Hello %(name)s!
Your age is %(age)s
"""
>>> values = {'name': 'Bob', 'age': 40}       # Build up values to substitute
>>> print(reply % values)                     # Perform substitutions
```
> Greetings...
> Hello Bob!
> Your age is 40

### String Formatting Method Calls

The string object’s format method, available in Python 2.6, 2.7, and 3.X, is based on normal function call syntax, instead of an expression. Specifically, it uses the subject string as a template, and takes any number of arguments that represent values to be substituted according to the template.

```python
>>> template = '{0}, {1} and {2}'                             # By position
>>> template.format('spam', 'ham', 'eggs')
```
> 'spam, ham and eggs'

```python
>>> template = '{motto}, {pork} and {food}'                   # By keyword
>>> template.format(motto='spam', pork='ham', food='eggs')
```
> 'spam, ham and eggs'

```python
>>> template = '{motto}, {0} and {food}'                      # By both
>>> template.format('ham', motto='spam', food='eggs')
```
> 'spam, ham and eggs'

```python
>>> template = '{}, {} and {}'                                # By relative position
>>> template.format('spam', 'ham', 'eggs')                    # New in 3.1 and 2.7
```
> 'spam, ham and eggs'

Finally, Python 2.6 and 3.0 also introduced a new built-in format function, which can be used to format a single item. It’s a more concise alternative to the string format method, and is roughly similar to formatting a single item with the % formatting expression:

```python
>>> '{0:.2f}'.format(1.2345)                         # String method
``` 
> '1.23'

```python
>>> format(1.2345, '.2f')                            # Built-in function
```
> '1.23'

```python
>>> '%.2f' % 1.2345                                  # Expression
```
> '1.23'

### Why the Format Method?

1. Has a handful of extra features not found in the % expression itself (though % can use alternatives)
2. Has more flexible value reference syntax (though it may be overkill, and % often has equivalents)
3. Can make substitution value references more explicit (though this is now optional)
4. Trades an operator for a more mnemonic method name (though this is also more verbose)
5. Does not allow different syntax for single and multiple values (though practice suggests this is trivial)
6. As a function can be used in places an expression cannot (though a one-line function renders this moot)

* * *

# Test Your Knowledge

### Q1 - Can the string find method be used to search a list?

```
No, because methods are always type-specific; that is, they only work on a single data type. 
Expressions like X+Y and built-in functions like len(X) are generic, though, and may work on a 
variety of types. In this case, for instance, the in membership expression has a similar effect 
as the string find, but it can be used to search both strings and lists. In Python 3.X, there 
is some attempt to group methods by categories (for example, the mutable sequence types list 
and bytearray have similar method sets), but methods are still more type-specific than other 
operation sets.
```

### Q2 - Can a string slice expression be used on a list?

```
Yes. Unlike methods, expressions are generic and apply to many types. In this case, the slice 
expression is really a sequence operation—it works on any type of sequence object, including 
strings, lists, and tuples. The only difference is that when you slice a list, you get back 
a new list.
```

### Q3 - How would you convert a character to its ASCII integer code? How would you convert the other way, from an integer to a character?

```
The built-in ord(S) function converts from a one-character string to an integer character 
code; chr(I) converts from the integer code back to a string. Keep in mind, though, that 
these integers are only ASCII codes for text whose characters are drawn only from ASCII 
character set. In the Unicode model, text strings are really sequences of Unicode code 
point identifying integers, which may fall outside the 7-bit range of numbers reserved 
by ASCII
```

### Q4 - How might you go about changing a string in Python?

```
Strings cannot be changed; they are immutable. However, you can achieve a similar effect 
by creating a new string—by concatenating, slicing, running formatting expressions, or 
using a method call like replace—and then assigning the result back to the original 
variable name.
```

### Q5 - Given a string S with the value "s,pa,m", name two ways to extract the two characters in the middle.

```
You can slice the string using S[2:4], or split on the comma and index the string using 
S.split(',')[1]. Try these interactively to see for yourself.
```

### Q6 - How many characters are there in the string "a\nb\x1f\000d"?

```
Six. The string "a\nb\x1f\000d" contains the characters a, newline (\n), b, binary 31 
(a hex escape \x1f), binary 0 (an octal escape \000), and d. Pass the string to the 
built-in len function to verify this, and print each of its character’s ord results to
see the actual code point (identifying number) values. See Table 7-2 for more details
on escapes.
```

### Q7 - Why might you use the string module instead of string method calls?

```
You should never use the string module instead of string object method calls today —it’s 
deprecated, and its calls are removed completely in Python 3.X. The only valid reason 
for using the string module at all today is for its other tools, such as predefined 
constants. You might also see it appear in what is now very old and dusty Python code.
```
