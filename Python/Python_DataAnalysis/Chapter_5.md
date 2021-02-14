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
> 0    4
> 1    7
> 2   -5
> 3    3
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
> d    4
> b    7
> a   -5
> c    3
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
> d    6
> b    7
> c    3
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
> Ohio      35000
> Oregon    16000
> Texas     71000
> Utah       5000
> dtype: int64

The isnull and notnull functions in pandas should be used to detect missing data :

```python
In [32]: pd.isnull(obj3)
``` 
> Ohio          False
> Oregon        False
> Texas         False
> dtype: bool

A useful Series feature for many applications is that it automatically aligns by index label in arithmetic operations :

```python
In [37]: obj3 + obj3
```
> Ohio      70000
> Oregon    32000
> Texas    142000
> Utah      10000

Both the Series object itself and its index have a name attribute, which integrates with other key areas of pandas functionality :

```python
In [38]: obj3.name = 'population'
In [39]: obj3.index.name = 'state'
In [40]: obj3
```
> state
> Ohio      35000
> Oregon    16000
> Texas     71000
> Utah       5000
> Name: population, dtype: float64

### DataFrame

