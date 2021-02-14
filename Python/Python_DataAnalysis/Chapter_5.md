---
layout: default
---

# Getting Started with pandas

## Pandas

It contains data structures and data manipulation tools designed to make data cleaning and analysis fast and easy in Python. pandas is often used in tandem with numerical computing tools like NumPy and SciPy, analytical libraries like statsmodels and scikit-learn, and data visualization libraries like matplotlib. pandas adopts significant parts of NumPy’s idiomatic style of array-based computing, especially array-based functions and a preference for data processing without for loops. While pandas adopts many coding idioms from NumPy, the biggest difference is that pandas is designed for working with tabular or heterogeneous data. NumPy, by contrast, is best suited for working with homogeneous numerical array data.

```python
import pandas as pd
```

## Introduction to pandas Data Structures

To get started with pandas, you will need to get comfortable with its two workhorse data structures: Series and DataFrame. While they are not a universal solution for every problem, they provide a solid, easy-to-use basis for most applications.

### Series

A Series is a one-dimensional array-like object containing a sequence of values (of similar types to NumPy types) and an associated array of data labels, called its index. The simplest Series is formed from only an array of data :

```python
In [11]: obj = pd.Series([4, 7, -5, 3])
In [12]: obj
```
> 0    4<br>
> 1    7<br>
> 2   -5<br>
> 3    3<br>
> dtype: int64

You can get the array representation and index object of the Series via its values and index attributes, respectively :

```python
In [13]: obj.values
```
> array([ 4,  7, -5,  3])

```python
In [14]: obj.index  # like range(4)
```
> RangeIndex(start=0, stop=4, step=1)

Often it will be desirable to create a Series with an index identifying each data point with a label :

```python
In [15]: obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
In [16]: obj2
```
> d    4<br>
> b    7<br>
> a   -5<br>
> c    3<br>
> dtype: int64

Compared with NumPy arrays, you can use labels in the index when selecting single values or a set of values :

```python
In [18]: obj2['a']
```
> -5

Using NumPy functions or NumPy-like operations, such as filtering with a boolean array, scalar multiplication, or applying math functions, will preserve the index-value link :

```python
In [21]: obj2[obj2 > 0]
```
> d    6<br>
> b    7<br>
> c    3<br>
> dtype: int64

Another way to think about a Series is as a fixed-length, ordered dict, as it is a mapping of index values to data values. It can be used in many contexts where you might use a dict :

```python
In [24]: 'b' in obj2
```
> True

Should you have data contained in a Python dict, you can create a Series from it by passing the dict :

```python
In [26]: sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
In [27]: obj3 = pd.Series(sdata)
In [28]: obj3
```
> Ohio      35000<br>
> Oregon    16000<br>
> Texas     71000<br>
> Utah       5000<br>
> dtype: int64

The isnull and notnull functions in pandas should be used to detect missing data :

```python
In [32]: pd.isnull(obj3)
``` 
> Ohio          False<br>
> Oregon        False<br>
> Texas         False<br>
> dtype: bool

A useful Series feature for many applications is that it automatically aligns by index label in arithmetic operations :

```python
In [37]: obj3 + obj3
```
> Ohio      70000<br>
> Oregon    32000<br>
> Texas    142000<br>
> Utah      10000<br>

Both the Series object itself and its index have a name attribute, which integrates with other key areas of pandas functionality :

```python
In [38]: obj3.name = 'population'
In [39]: obj3.index.name = 'state'
In [40]: obj3
```
> state
> Ohio      35000<br>
> Oregon    16000<br>
> Texas     71000<br>
> Utah       5000<br>
> Name: population, dtype: float64

### DataFrame

A DataFrame represents a rectangular table of data and contains an ordered collection of columns, each of which can be a different value type (numeric, string, boolean, etc.). The DataFrame has both a row and column index; it can be thought of as a dict of Series all sharing the same index. There are many ways to construct a DataFrame, though one of the most common is from a dict of equal-length lists or NumPy arrays :

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```

The resulting DataFrame will have its index assigned automatically as with Series, and the columns are placed in sorted order :

```python
In [45]: frame
```
> pop   state  year<br>
> 0  1.5    Ohio  2000<br>
> 1  1.7    Ohio  2001<br>
> 2  3.6    Ohio  2002<br>
> 3  2.4  Nevada  2001<br>
> 4  2.9  Nevada  2002<br>
> 5  3.2  Nevada  2003

For large DataFrames, the head method selects only the first five rows :

```python
In [46]: frame.head()
```
> pop   state  year<br>
> 0  1.5    Ohio  2000<br>
> 1  1.7    Ohio  2001<br>
> 2  3.6    Ohio  2002<br>
> 3  2.4  Nevada  2001<br>
> 4  2.9  Nevada  2002

If you specify a sequence of columns, the DataFrame’s columns will be arranged in that order :

```python
In [47]: pd.DataFrame(data, columns=['year', 'state', 'pop'])
``` 
> year   state  pop<br>
> 0  2000    Ohio  1.5<br>
> 1  2001    Ohio  1.7<br>
> 2  2002    Ohio  3.6<br>
> 3  2001  Nevada  2.4<br>
> 4  2002  Nevada  2.9<br>
> 5  2003  Nevada  3.2

A column in a DataFrame can be retrieved as a Series either by dict-like notation or by attribute :

```python
In [51]: frame2['state']
In [52]: frame2.year
```

Columns can be modified by assignment. For example, the empty 'debt' column could be assigned a scalar value or an array of values :

```python
In [54]: frame2['debt'] = 16.5
In [55]: frame2
```
> year   state  pop  debt<br>
> one    2000    Ohio  1.5  16.5<br>
> two    2001    Ohio  1.7  16.5<br>
> three  2002    Ohio  3.6  16.5<br>
> four   2001  Nevada  2.4  16.5<br>
> five   2002  Nevada  2.9  16.5<br>
> six    2003  Nevada  3.2  16.5

Assigning a column that doesn’t exist will create a new column. The del keyword will delete columns as with a dict.

```
In [61]: frame2['eastern'] = frame2.state == 'Ohio'
In [62]: frame2
```
> year   state  pop  debt  eastern<br>
> one    2000    Ohio  1.5   NaN     True<br>
> two    2001    Ohio  1.7  -1.2     True<br>
> three  2002    Ohio  3.6   NaN     True<br>
> four   2001  Nevada  2.4  -1.5    False<br>
> five   2002  Nevada  2.9  -1.7    False<br>
> six    2003  Nevada  3.2   NaN    False

The del method can then be used to remove this column :

```python
In [63]: del frame2['eastern']
In [64]: frame2.columns
```
> Index(['year', 'state', 'pop', 'debt'], dtype='object')

Another common form of data is a nested dict of dicts :

```python
In [65]: pop = {'Nevada': {2001: 2.4, 2002: 2.9},
   ....:        'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
```

If the nested dict is passed to the DataFrame, pandas will interpret the outer dict keys as the columns and the inner keys as the row indices :

```python
In [66]: frame3 = pd.DataFrame(pop)
In [67]: frame3
```
> Nevada  Ohio<br>
> 2000     NaN   1.5<br>
> 2001     2.4   1.7<br>
> 2002     2.9   3.6

Possible data inputs to DataFrame constructor

| Type        | Notes          |
|:-------------|:------------------|
| 2D ndarray | A matrix of data, passing optional row and column labels |
| dict of arrays, lists, or tuples | Each sequence becomes a column in the DataFrame; all sequences must be the same length |
| NumPy structured/record array | Treated as the “dict of arrays” case |
| dict of Series | Each value becomes a column; indexes from each Series are unioned together to form the result’s row index if no explicit index is passed |
| dict of dicts | Each inner dict becomes a column; keys are unioned to form the row index as in the “dict of Series” case |
| List of dicts or Series | Each item becomes a row in the DataFrame; union of dict keys or Series indexes become the DataFrame’s column labels |
| List of lists or tuples | Treated as the “2D ndarray” case |
| Another DataFrame | The DataFrame’s indexes are used unless different ones are passed |
| NumPy MaskedArray | Like the “2D ndarray” case except masked values become NA/missing in the DataFrame result |

### Index Objects

pandas’s Index objects are responsible for holding the axis labels and other metadata (like the axis name or names). Any array or other sequence of labels you use when constructing a Series or DataFrame is internally converted to an Index :

```python
In [76]: obj = pd.Series(range(3), index=['a', 'b', 'c'])
In [77]: index = obj.index
In [78]: index
``` 
> Index(['a', 'b', 'c'], dtype='object')

```python
In [79]: index[1:]
```
> Index(['b', 'c'], dtype='object')

Index objects are immutable and thus can’t be modified by the user. In addition to being array-like, an Index also behaves like a fixed-size set. Unlike Python sets, a pandas Index can contain duplicate labels. Selections with duplicate labels will select all occurrences of that label. Some index methods and properties :

| Method        | Description          |
|:-------------|:------------------|
| append | Concatenate with additional Index objects, producing a new Index |
| difference | Compute set difference as an Index |
| intersection | Compute set intersection |
| union | Compute set union |
| isin | Compute boolean array indicating whether each value is contained in the passed collection |
| delete | Compute new Index with element at index i deleted |
| drop | Compute new Index by deleting passed values |
| insert | Compute new Index by inserting element at index i |
| is_monotonic | Returns True if each element is greater than or equal to the previous element |
| is_unique | Returns True if the Index has no duplicate values |
| unique | Compute the array of unique values in the Index |

## Essential Functionality

### Reindexing

An important method on pandas objects is reindex, which means to create a new object with the data conformed to a new index. Consider an example :

```python
In [91]: obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
In [92]: obj
```
> d    4.5<br>
> b    7.2<br>
> a   -5.3<br>
> c    3.6<br>
> dtype: float64

Calling reindex on this Series rearranges the data according to the new index, introducing missing values if any index values were not already present :

```python
In [93]: obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
In [94]: obj2
```
> a   -5.3<br>
> b    7.2<br>
> c    3.6<br>
> d    4.5<br>
> e    NaN<br>
> dtype: float64

For ordered data like time series, it may be desirable to do some interpolation or filling of values when reindexing. The method option allows us to do this, using a method such as ffill, which forward-fills the values :

```python
In [95]: obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
In [96]: obj3
```
> 0      blue<br>
> 2    purple<br>
> 4    yellow<br>
> dtype: object

```python
In [97]: obj3.reindex(range(6), method='ffill')
```
> 0      blue<br>
> 1      blue<br>
> 2    purple<br>
> 3    purple<br>
> 4    yellow<br>
> 5    yellow<br>
> dtype: object

Reindex Function arguments :

| Method        | Description          |
|:-------------|:------------------|
| index | New sequence to use as index. Can be Index instance or any other sequence-like Python data structure. An Index will be used exactly as is without any copying. |
| method | Interpolation (fill) method; 'ffill' fills forward, while 'bfill' fills backward. |
| fill_value | Substitute value to use when introducing missing data by reindexing. |
| limit | When forward- or backfilling, maximum size gap (in number of elements) to fill. |
| tolerance | When forward- or backfilling, maximum size gap (in absolute numeric distance) to fill for inexact matches. |
| level | Match simple Index on level of MultiIndex; otherwise select subset of. |
| copy | If True, always copy underlying data even if new index is equivalent to old index; if False, do not copy the data when the indexes are equivalent. |

### Dropping Entries from an Axis
