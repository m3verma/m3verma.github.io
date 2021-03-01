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

### Summary Statistics by Level

