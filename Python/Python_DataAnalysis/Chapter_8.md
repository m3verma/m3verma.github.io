---
layout: default
---

# Data Wrangling: Join, Combine, and Reshape

## Hierarchical Indexing

Hierarchical indexing is an important feature of pandas that enables you to have multiple (two or more) index levels on an axis. Somewhat abstractly, it provides a way for you to work with higher dimensional data in a lower dimensional form. Let’s start with a simple example; create a Series with a list of lists (or arrays) as the index :

```python
In [9]: data = pd.Series(np.random.randn(9),
   ...:                  index=[['a', 'a', 'a', 'b', 'b', 'c', 'c', 'd', 'd'],
   ...:                         [1, 2, 3, 1, 3, 1, 2, 2, 3]])
In [10]: data
```
> a  1   -0.204708<br>
>    2    0.478943<br>
>    3   -0.519439<br>
> b  1   -0.555730<br>
>    3    1.965781<br>
> c  1    1.393406<br>
>    2    0.092908<br>
> d  2    0.281746<br>
>    3    0.769023<br>
> dtype: float64<br>

With a hierarchically indexed object, so-called partial indexing is possible, enabling you to concisely select subsets of the data :

```python
In [12]: data['b']
```
> 1   -0.555730<br>
> 3    1.965781<br>
> dtype: float64<br>

Hierarchical indexing plays an important role in reshaping data and group-based operations like forming a pivot table. For example, you could rearrange the data into a DataFrame using its unstack method :

```python
In [16]: data.unstack()
```
> 1         2         3<br>
> a -0.204708  0.478943 -0.519439<br>
> b -0.555730       NaN  1.965781<br>
> c  1.393406  0.092908       NaN<br>
> d       NaN  0.281746  0.769023<br>

stack and unstack will be explored in more detail later in this chapter. With a DataFrame, either axis can have a hierarchical index. 

```python
In [18]: frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
   ....:                      index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
   ....:                      columns=[['Ohio', 'Ohio', 'Colorado'],
   ....:                               ['Green', 'Red', 'Green']])
In [20]: frame.index.names = ['key1', 'key2']
In [21]: frame.columns.names = ['state', 'color']
In [19]: frame
```
> state      Ohio     Colorado<br>
> color     Green Red    Green<br>
> key1 key2                   <br>
> a    1        0   1        2<br>
>      2        3   4        5<br>
> b    1        6   7        8<br>
>      2        9  10       11<br>
  
 ### Reordering and Sorting Levels
 
At times you will need to rearrange the order of the levels on an axis or sort the data by the values in one specific level. The swaplevel takes two level numbers or names and returns a new object with the levels interchanged (but the data is otherwise unaltered) :

```python
In [24]: frame.swaplevel('key1', 'key2')
```
> state      Ohio     Colorado<br>
> color     Green Red    Green<br>
> key2 key1                   <br>
> 1    a        0   1        2<br>
> 2    a        3   4        5<br>
> 1    b        6   7        8<br>
> 2    b        9  10       11<br>

sort_index, on the other hand, sorts the data using only the values in a single level. When swapping levels, it’s not uncommon to also use sort_index so that the result is lexicographically sorted by the indicated level :

```python
In [25]: frame.sort_index(level=1)
```
> state      Ohio     Colorado<br>
> color     Green Red    Green<br>
> key1 key2                   <br>
> a    1        0   1        2<br>
> b    1        6   7        8<br>
> a    2        3   4        5<br>
> b    2        9  10       11<br>

### Indexing with a DataFrame’s columns

It’s not unusual to want to use one or more columns from a DataFrame as the row index; alternatively, you may wish to move the row index into the DataFrame’s columns. Here’s an example DataFrame :

```python
In [29]: frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),
   ....:                       'c': ['one', 'one', 'one', 'two', 'two',
   ....:                             'two', 'two'],
   ....:                       'd': [0, 1, 2, 0, 1, 2, 3]})
In [30]: frame
```
> a  b    c  d<br>
> 0  0  7  one  0<br>
> 1  1  6  one  1<br>
> 2  2  5  one  2<br>
> 3  3  4  two  0<br>
> 4  4  3  two  1<br>
> 5  5  2  two  2<br>
> 6  6  1  two  3<br>

DataFrame’s set_index function will create a new DataFrame using one or more of its columns as the index :

```python
In [31]: frame2 = frame.set_index(['c', 'd'])
In [32]: frame2
```
> a  b<br>
> c   d      <br>
> one 0  0  7<br>
>    1  1  6<br>
>    2  2  5<br>
> two 0  3  4<br>
>    1  4  3<br>
>    2  5  2<br>
>    3  6  1<br>
    
## Combining and Merging Datasets

Data contained in pandas objects can be combined together in a number of ways :

1. pandas.merge connects rows in DataFrames based on one or more keys. This will be familiar to users of SQL or other relational databases, as it implements database join operations.
2. pandas.concat concatenates or “stacks” together objects along an axis.
3. The combine_first instance method enables splicing together overlapping data to fill in missing values in one object with values from another.

### Database-Style DataFrame Joins

Merge or join operations combine datasets by linking rows using one or more keys. These operations are central to relational databases (e.g., SQL-based). The merge function in pandas is the main entry point for using these algorithms on your data. Let’s start with a simple example :

```python
In [35]: df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
   ....:                     'data1': range(7)})
In [36]: df2 = pd.DataFrame({'key': ['a', 'b', 'd'],
   ....:                     'data2': range(3)})
```

This is an example of a many-to-one join; the data in df1 has multiple rows labeled a and b, whereas df2 has only one row for each value in the key column. Calling merge with these objects we obtain :

```python
In [39]: pd.merge(df1, df2)
```
> data1 key  data2<br>
> 0      0   b      1<br>
> 1      1   b      1<br>
> 2      6   b      1<br>
> 3      2   a      0<br>
> 4      4   a      0<br>
> 5      5   a      0<br>

Note that I didn’t specify which column to join on. If that information is not specified, merge uses the overlapping column names as the keys. It’s a good practice to specify explicitly, though :

```python
In [40]: pd.merge(df1, df2, on='key')
```
> data1 key  data2<br>
> 0      0   b      1<br>
> 1      1   b      1<br>
> 2      6   b      1<br>
> 3      2   a      0<br>
> 4      4   a      0<br>
> 5      5   a      0<br>

If the column names are different in each object, you can specify them separately :

```python
In [41]: df3 = pd.DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
   ....:                     'data1': range(7)})

In [42]: df4 = pd.DataFrame({'rkey': ['a', 'b', 'd'],
   ....:                     'data2': range(3)})
In [43]: pd.merge(df3, df4, left_on='lkey', right_on='rkey')
```
> data1 lkey  data2 rkey<br>
> 0      0    b      1    b<br>
> 1      1    b      1    b<br>
> 2      6    b      1    b<br>
> 3      2    a      0    a<br>
> 4      4    a      0    a<br>
> 5      5    a      0    a<br>

Different join types with how argument :

| Option        | Behavior          |
|:-------------|:------------------|
| 'inner' | Use only the key combinations observed in both tables |
| 'left' | Use all key combinations found in the left table |
| 'right' | Use all key combinations found in the right table |
| 'output' | Use all key combinations observed in both tables together |

Many-to-many merges have well-defined, though not necessarily intuitive, behavior. Here’s an example :

```python
In [45]: df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
   ....:                     'data1': range(6)})
In [46]: df2 = pd.DataFrame({'key': ['a', 'b', 'a', 'b', 'd'],
   ....:                     'data2': range(5)})
```

Many-to-many joins form the Cartesian product of the rows. Since there were three 'b' rows in the left DataFrame and two in the right one, there are six 'b' rows in the result. The join method only affects the distinct key values appearing in the result :

```python
In [50]: pd.merge(df1, df2, how='inner')
```
> data1 key  data2<br>
> 0      0   b      1<br>
> 1      0   b      3<br>
> 2      1   b      1<br>
> 3      1   b      3<br>
> 4      5   b      1<br>
> 5      5   b      3<br>
> 6      2   a      0<br>
> 7      2   a      2<br>
> 8      4   a      0<br>
> 9      4   a      2<br>

Different join types with how argument :

| Argument        | Description          |
|:-------------|:------------------|
| left | DataFrame to be merged on the left side. |
| right | DataFrame to be merged on the right side. |
| how | One of 'inner', 'outer', 'left', or 'right'; defaults to 'inner'. |
| on | Column names to join on. Must be found in both DataFrame objects. If not specified and no other join keys given, will use the intersection of the column names in left and right as the join keys. |
| left_on | Columns in left DataFrame to use as join keys. |
| right_on | Analogous to left_on for left DataFrame. |
| left_index | Use row index in left as its join key (or keys, if a MultiIndex). |
| right_index | Analogous to left_index. |
| sort | Sort merged data lexicographically by join keys; True by default |
| suffixes | Tuple of string values to append to column names in case of overlap; defaults to (_x, _y)  |
| copy | If False, avoid copying data into resulting data structure in some exceptional cases; by default always copies. |
| indicator | Adds a special column _merge that indicates the source of each row; values will be 'left_only', 'right_only', or 'both' based on the origin of the joined data in each row. |

### Merging on Index

In some cases, the merge key(s) in a DataFrame will be found in its index. In this case, you can pass left_index=True or right_index=True (or both) to indicate that the index should be used as the merge key :

```python
In [56]: left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],
   ....:                       'value': range(6)})
In [57]: right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
In [60]: pd.merge(left1, right1, left_on='key', right_index=True)
```
> key  value  group_val<br>
> 0   a      0        3.5<br>
> 2   a      2        3.5<br>
> 3   a      3        3.5<br>
> 1   b      1        7.0<br>
> 4   b      4        7.0<br>

DataFrame has a convenient join instance for merging by index. It can also be used to combine together many DataFrame objects having the same or similar indexes but non-overlapping columns.

```python
In [73]: left2.join(right2, how='outer')
```

### Concatenating Along an Axis

Another kind of data combination operation is referred to interchangeably as concatenation, binding, or stacking. NumPy’s concatenate function can do this with NumPy arrays:

```python
In [79]: arr = np.arange(12).reshape((3, 4))
In [80]: arr
```
> array([[ 0,  1,  2,  3],[ 4,  5,  6,  7],[ 8,  9, 10, 11]])

```python
In [81]: np.concatenate([arr, arr], axis=1)
```
> array([[ 0,  1,  2,  3,  0,  1,  2,  3],[ 4,  5,  6,  7,  4,  5,  6,  7],[ 8,  9, 10, 11,  8,  9, 10, 11]])

In the context of pandas objects such as Series and DataFrame, having labeled axes enable you to further generalize array concatenation. In particular, you have a number of additional things to think about :

1. If the objects are indexed differently on the other axes, should we combine the distinct elements in these axes or use only the shared values (the intersection)?
2. Do the concatenated chunks of data need to be identifiable in the resulting object?
3. Does the “concatenation axis” contain data that needs to be preserved? In many cases, the default integer labels in a DataFrame are best discarded during concatenation.

The concat function in pandas provides a consistent way to address each of these concerns. I’ll give a number of examples to illustrate how it works. Suppose we have three Series with no index overlap :

```python
In [82]: s1 = pd.Series([0, 1], index=['a', 'b'])
In [83]: s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
In [84]: s3 = pd.Series([5, 6], index=['f', 'g'])
```

Calling concat with these objects in a list glues together the values and indexes :

```
In [85]: pd.concat([s1, s2, s3])
``` 
> a    0<br>
> b    1<br>
> c    2<br>
> d    3<br>
> e    4<br>
> f    5<br>
> g    6<br>
> dtype: int64<br>

The same logic extends to DataFrame objects :

```python
In [96]: df1 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'],
   ....:                    columns=['one', 'two'])
In [97]: df2 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'],
   ....:                    columns=['three', 'four'])
In [100]: pd.concat([df1, df2], axis=1, keys=['level1', 'level2'])
```
> level1     level2<br>     
> one two  three four<br>
> a      0   1    5.0  6.0<br>
> b      2   3    NaN  NaN<br>
> c      4   5    7.0  8.0<br>

If you pass a dict of objects instead of a list, the dict’s keys will be used for the keys option :

```python
In [101]: pd.concat({'level1': df1, 'level2': df2}, axis=1)
``` 
> level1     level2<br>     
> one two  three four<br>
> a      0   1    5.0  6.0<br>
> b      2   3    NaN  NaN<br>
> c      4   5    7.0  8.0<br>

concat function arguments :

| Argument        | Description          |
|:-------------|:------------------|
| objs | List or dict of pandas objects to be concatenated; this is the only required argument |
| axis | Axis to concatenate along; defaults to 0 (along rows) |
| join | Either 'inner' or 'outer' ('outer' by default); whether to intersection (inner) or union (outer) together indexes along the other axes |
| join_axes | Specific indexes to use for the other n–1 axes instead of performing union/intersection logic |
| keys | Values to associate with objects being concatenated, forming a hierarchical index along the concatenation axis; can either be a list or array of arbitrary values, an array of tuples, or a list of arrays (if multiple-level arrays passed in levels) |
| levels | Specific indexes to use as hierarchical index level or levels if keys passed |
| names | Names for created hierarchical levels if keys and/or levels passed |
| verify_integrity | Check new axis in concatenated object for duplicates and raise exception if so; by default (False) allows duplicates |
| ignore_index | Do not preserve indexes along concatenation axis, instead producing a new range(total_length) index |

### Combining Data with Overlap

