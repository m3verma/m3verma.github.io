---
layout: default
---

# Data Cleaning and Preparation

During the course of doing data analysis and modeling, a significant amount of time is spent on data preparation: loading, cleaning, transforming, and rearranging. Such tasks are often reported to take up 80% or more of an analyst’s time.

## Handling Missing Data

Missing data occurs commonly in many data analysis applications. One of the goals of pandas is to make working with missing data as painless as possible. For example, all of the descriptive statistics on pandas objects exclude missing data by default. For numeric data, pandas uses the floating-point value NaN (Not a Number) to represent missing data. We call this a sentinel value that can be easily detected :

```python
In [10]: string_data = pd.Series(['aardvark', 'artichoke', np.nan, 'avocado'])
In [11]: string_data
```
> 0     aardvark<br>
> 1    artichoke<br>
> 2          NaN<br>
> 3      avocado<br>
> dtype: object<br>

The built-in Python None value is also treated as NA in object arrays :

```python
In [13]: string_data[0] = None
In [14]: string_data.isnull()
```
> 0     True<br>
> 1    False<br>
> 2     True<br>
> 3    False<br>
> dtype: bool<br>

NA handling methods :

| Argument        | Description          |
|:-------------|:------------------|
| dropna | Filter axis labels based on whether values for each label have missing data, with varying thresholds for how much missing data to tolerate. |
| fillna | Fill in missing data with some value or using an interpolation method such as 'ffill' or 'bfill'. |
| isnull | Return boolean values indicating which values are missing/NA. |
| notnull | Negation of isnull. |

### Filtering Out Missing Data

There are a few ways to filter out missing data. While you always have the option to do it by hand using pandas.isnull and boolean indexing, the dropna can be helpful. On a Series, it returns the Series with only the non-null data and index values :

```python
In [15]: from numpy import nan as NA
In [16]: data = pd.Series([1, NA, 3.5, NA, 7])
In [17]: data.dropna()
```
> 0    1.0<br>
> 2    3.5<br>
> 4    7.0<br>
> dtype: float64<br>

With DataFrame objects, things are a bit more complex. You may want to drop rows or columns that are all NA or only those containing any NAs. dropna by default drops any row containing a missing value :

```python
In [19]: data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA],
   ....:                      [NA, NA, NA], [NA, 6.5, 3.]])
In [20]: cleaned = data.dropna()
In [22]: cleaned
```
> 0    1    2<br>
> 0  1.0  6.5  3.0<br>

Passing how='all' will only drop rows that are all NA. To drop columns in the same way, pass axis=1.

### Filling In Missing Data

Rather than filtering out missing data (and potentially discarding other data along with it), you may want to fill in the “holes” in any number of ways. For most purposes, the fillna method is the workhorse function to use. Calling fillna with a constant replaces missing values with that value :

```python
In [33]: df.fillna(0)
```
> 0         1         2<br>
> 0 -0.204708  0.000000  0.000000<br>
> 1 -0.555730  0.000000  0.000000<br>
> 2  0.092908  0.000000  0.769023<br>
> 3  1.246435  0.000000 -1.296221<br>

Calling fillna with a dict, you can use a different fill value for each column :

```python
In [34]: df.fillna({1: 0.5, 2: 0})
```
> 0         1         2<br>
> 0 -0.204708  0.500000  0.000000<br>
> 1 -0.555730  0.500000  0.000000<br>
> 2  0.092908  0.500000  0.769023<br>
> 3  1.246435  0.500000 -1.296221<br>

fillna function arguments :

| Argument        | Description          |
|:-------------|:------------------|
| value | Scalar value or dict-like object to use to fill missing values |
| method | Interpolation; by default 'ffill' if function called with no other arguments |
| axis | Axis to fill on; default axis=0 |
| inplace | Modify the calling object without producing a copy |
| limit | For forward and backward filling, maximum number of consecutive periods to fill |

## Data Transformation

### Removing Duplicates

Duplicate rows may be found in a DataFrame for any number of reasons. Here is an example :

```python
In [45]: data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],
   ....:                      'k2': [1, 1, 2, 3, 3, 4, 4]})
In [46]: data
```
> k1  k2<br>
> 0  one   1<br>
> 1  two   1<br>
> 2  one   2<br>
> 3  two   3<br>
> 4  one   3<br>
> 5  two   4<br>
> 6  two   4<br>

The DataFrame method duplicated returns a boolean Series indicating whether each row is a duplicate (has been observed in a previous row) or not :

```python
In [47]: data.duplicated()
```
> 0    False<br>
> 1    False<br>
> 2    False<br>
> 3    False<br>
> 4    False<br>
> 5    False<br>
> 6     True<br>
> dtype: bool<br>

Alternatively, you can specify any subset of them to detect duplicates. Suppose we had an additional column of values and wanted to filter duplicates only based on the 'k1' column :

```python
In [49]: data['v1'] = range(7)
In [50]: data.drop_duplicates(['k1'])
```
> k1  k2  v1<br>
> 0  one   1   0<br>
> 1  two   1   1<br>

### Transforming Data Using a Function or Mapping

For many datasets, you may wish to perform some transformation based on the values in an array, Series, or column in a DataFrame. Consider the following hypothetical data collected about various kinds of meat :

```python
In [52]: data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon',
   ....:                               'Pastrami', 'corned beef', 'Bacon',
   ....:                               'pastrami', 'honey ham', 'nova lox'],
   ....:                      'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
In [53]: data
```
> food  ounces<br>
> 0        bacon     4.0<br>
> 1  pulled pork     3.0<br>
> 2        bacon    12.0<br>
> 3     Pastrami     6.0<br>
> 4  corned beef     7.5<br>
> 5        Bacon     8.0<br>
> 6     pastrami     3.0<br>
> 7    honey ham     5.0<br>
> 8     nova lox     6.0<br>

Suppose you wanted to add a column indicating the type of animal that each food came from. Let’s write down a mapping of each distinct meat type to the kind of animal :

```python
meat_to_animal = {
  'bacon': 'pig',
  'pulled pork': 'pig',
  'pastrami': 'cow',
  'corned beef': 'cow',
  'honey ham': 'pig',
  'nova lox': 'salmon'
}
```

The map method on a Series accepts a function or dict-like object containing a mapping, but here we have a small problem in that some of the meats are capitalized and others are not. Thus, we need to convert each value to lowercase using the str.lower Series method :

```python
In [55]: lowercased = data['food'].str.lower()
In [56]: lowercased
```
> 0          bacon<br>
> 1    pulled pork<br>
> 2          bacon<br>
> 3       pastrami<br>
> 4    corned beef<br>
> 5          bacon<br>
> 6       pastrami<br>
> 7      honey ham<br>
> 8       nova lox<br>
> Name: food, dtype: object<br>

```python
In [57]: data['animal'] = lowercased.map(meat_to_animal)
In [58]: data
```
> food  ounces  animal<br>
> 0        bacon     4.0     pig<br>
> 1  pulled pork     3.0     pig<br>
> 2        bacon    12.0     pig<br>
> 3     Pastrami     6.0     cow<br>
> 4  corned beef     7.5     cow<br>
> 5        Bacon     8.0     pig<br>
> 6     pastrami     3.0     cow<br>
> 7    honey ham     5.0     pig<br>
> 8     nova lox     6.0  salmon<br>

### Replacing Values

Filling in missing data with the fillna method is a special case of more general value replacement. As you’ve already seen, map can be used to modify a subset of values in an object but replace provides a simpler and more flexible way to do so. Let’s consider this Series :

```python
In [60]: data = pd.Series([1., -999., 2., -999., -1000., 3.])
In [61]: data
```
> 0       1.0<br>
> 1    -999.0<br>
> 2       2.0<br>
> 3    -999.0<br>
> 4   -1000.0<br>
> 5       3.0<br>
> dtype: float64<br>

The -999 values might be sentinel values for missing data. To replace these with NA values that pandas understands, we can use replace, producing a new Series (unless you pass inplace=True) :

```python
In [62]: data.replace(-999, np.nan)
```
> 0       1.0<br>
> 1       NaN<br>
> 2       2.0<br>
> 3       NaN<br>
> 4   -1000.0<br>
> 5       3.0<br>
> dtype: float64<br>

### Renaming Axis Indexes

Like values in a Series, axis labels can be similarly transformed by a function or mapping of some form to produce new, differently labeled objects. You can also modify the axes in-place without creating a new data structure. Here’s a simple example :

```python
In [66]: data = pd.DataFrame(np.arange(12).reshape((3, 4)),
   ....:                     index=['Ohio', 'Colorado', 'New York'],
   ....:                     columns=['one', 'two', 'three', 'four'])
```

Like a Series, the axis indexes have a map method :

```python
In [67]: transform = lambda x: x[:4].upper()
In [68]: data.index.map(transform)
```
> Index(['OHIO', 'COLO', 'NEW '], dtype='object')

```python
In [69]: data.index = data.index.map(transform)
In [70]: data
```
> one  two  three  four<br>
> OHIO    0    1      2     3<br>
> COLO    4    5      6     7<br>
> NEW     8    9     10    11<br>

If you want to create a transformed version of a dataset without modifying the original, a useful method is rename :

```python
In [71]: data.rename(index=str.title, columns=str.upper)
```
> ONE  TWO  THREE  FOUR<br>
> Ohio    0    1      2     3<br>
> Colo    4    5      6     7<br>
> New     8    9     10    11<br>

### Discretization and Binning

Continuous data is often discretized or otherwise separated into “bins” for analysis. Suppose you have data about a group of people in a study, and you want to group them into discrete age buckets. Let’s divide these into bins of 18 to 25, 26 to 35, 36 to 60, and finally 61 and older. To do so, you have to use cut, a function in pandas :

```python
In [75]: ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
In [76]: bins = [18, 25, 35, 60, 100]
In [77]: cats = pd.cut(ages, bins)
In [78]: cats
```
> [(18, 25], (18, 25], (18, 25], (25, 35], (18, 25], ..., (25, 35], (60, 100], (35, 60], (35, 60], (25, 35]]

You can also pass your own bin names by passing a list or array to the labels option.

### Detecting and Filtering Outliers

Filtering or transforming outliers is largely a matter of applying array operations. Consider a DataFrame with some normally distributed data :

```python
In [92]: data = pd.DataFrame(np.random.randn(1000, 4))
In [93]: data.describe()
```
> 0            1            2            3<br>
> count  1000.000000  1000.000000  1000.000000  1000.000000<br>
> mean      0.049091     0.026112    -0.002544    -0.051827<br>
> std       0.996947     1.007458     0.995232     0.998311<br>
> min      -3.645860    -3.184377    -3.745356    -3.428254<br>
> 25%      -0.599807    -0.612162    -0.687373    -0.747478<br>
> 50%       0.047101    -0.013609    -0.022158    -0.088274<br>
> 75%       0.756646     0.695298     0.699046     0.623331<br>
> max       2.653656     3.525865     2.735527     3.366626<br>

Suppose you wanted to find values in one of the columns exceeding 3 in absolute value :

```python
In [94]: col = data[2]
In [95]: col[np.abs(col) > 3]
```
> 41    -3.399312<br>
> 136   -3.745356<br>
> Name: 2, dtype: float64<br>

To select all rows having a value exceeding 3 or –3, you can use the any method on a boolean DataFrame :

```python
In [96]: data[(np.abs(data) > 3).any(1)]
```
> 0         1         2         3<br>
> 41   0.457246 -0.025907 -3.399312 -0.974657<br>
> 60   1.951312  3.260383  0.963301  1.201206<br>
> 136  0.508391 -0.196713 -3.745356 -1.520113<br>
> 235 -0.242459 -3.056990  1.918403 -0.578828<br>
> 258  0.682841  0.326045  0.425384 -3.428254<br>
> 322  1.179227 -3.184377  1.369891 -1.074833<br>
> 544 -3.548824  1.553205 -2.186301  1.277104<br>
> 635 -0.578093  0.193299  1.397822  3.366626<br>
> 782 -0.207434  3.525865  0.283070  0.544635<br>
> 803 -3.645860  0.255475 -0.549574 -1.907459<br>

### Computing Indicator/Dummy Variables

Another type of transformation for statistical modeling or machine learning applications is converting a categorical variable into a “dummy” or “indicator” matrix. If a column in a DataFrame has k distinct values, you would derive a matrix or DataFrame with k columns containing all 1s and 0s. pandas has a get_dummies function for doing this, though devising one yourself is not difficult. Let’s return to an earlier example DataFrame :

```python
In [109]: df = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
   .....:                    'data1': range(6)})
In [110]: pd.get_dummies(df['key'])
```
> a  b  c<br>
> 0  0  1  0<br>
> 1  0  1  0<br>
> 2  1  0  0<br>
> 3  0  0  1<br>
> 4  1  0  0<br>
> 5  0  1  0<br>

## String Manipulation

Python has long been a popular raw data manipulation language in part due to its ease of use for string and text processing. Most text operations are made simple with the string object’s built-in methods. For more complex pattern matching and text manipulations, regular expressions may be needed. pandas adds to the mix by enabling you to apply string and regular expressions concisely on whole arrays of data, additionally handling the annoyance of missing data.

### String Object Methods

In many string munging and scripting applications, built-in string methods are sufficient. As an example, a comma-separated string can be broken into pieces with split :

```python
In [134]: val = 'a,b,  guido'
In [135]: val.split(',')
``` 
> ['a', 'b', '  guido']

Relatedly, count returns the number of occurrences of a particular substring :

```python
In [145]: val.count(',')
```
> 2

replace will substitute occurrences of one pattern for another. It is commonly used to delete patterns, too, by passing an empty string :

```python
In [146]: val.replace(',', '::')
```
> 'a::b::  guido'

```python
In [147]: val.replace(',', '')
```
> 'ab  guido'

Python built-in string methods :

| Argument        | Description          |
|:-------------|:------------------|
| count | Return the number of non-overlapping occurrences of substring in the string. |
| endswith | Returns True if string ends with suffix. |
| startswith | Returns True if string starts with prefix. |
| join | Use string as delimiter for concatenating a sequence of other strings. |
| index | Return position of first character in substring if found in the string; raises ValueError if not found. |
| find | Return position of first character of first occurrence of substring in the string; like index, but returns –1 if not found. |
| rfind | Return position of first character of last occurrence of substring in the string; returns –1 if not found. |
| replace | Replace occurrences of string with another string. |
| strip, rstrip, lstrip | Trim whitespace, including newlines; equivalent to x.strip() (and rstrip, lstrip, respectively) for each element. |
| split | Break string into list of substrings using passed delimiter. |
| lower | Convert alphabet characters to lowercase. |
| upper | Convert alphabet characters to uppercase. |
| casefold | Convert characters to lowercase, and convert any region-specific variable character combinations to a common comparable form. |
| ljust, rjust | Left justify or right justify, respectively; pad opposite side of string with spaces (or some other fill character) to return a string with a minimum width. |

### Regular Expressions

Regular expressions provide a flexible way to search or match (often more complex) string patterns in text. A single expression, commonly called a regex, is a string formed according to the regular expression language. Python’s built-in re module is responsible for applying regular expressions to strings; I’ll give a number of examples of its use here. Suppose we wanted to split a string with a variable number of whitespace characters (tabs, spaces, and newlines). The regex describing one or more whitespace characters is \s+ :

```python
In [148]: import re
In [149]: text = "foo    bar\t baz  \tqux"
In [150]: re.split('\s+', text)
```
> ['foo', 'bar', 'baz', 'qux']

regex.match returns None, as it only will match if the pattern occurs at the start of the string :

```python
In [159]: print(regex.match(text))
```
> None

Regular expression methods :

| Argument        | Description          |
|:-------------|:------------------|
| findall | Return all non-overlapping matching patterns in a string as a list |
| finditer | Like findall, but returns an iterator |
| match | Match pattern at start of string and optionally segment pattern components into groups; if the pattern matches, returns a match object, and otherwise None |
| search | Scan string for match to pattern; returning a match object if so; unlike match, the match can be anywhere in the string as opposed to only at the beginning |
| split | Break string into pieces at each occurrence of pattern |
| sub, subn | Replace all (sub) or first n occurrences (subn) of pattern in string with replacement expression; use symbols \1, \2, ... to refer to match group elements in the replacement string |
