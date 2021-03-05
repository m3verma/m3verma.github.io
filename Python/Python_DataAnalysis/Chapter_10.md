---
layout: default
---

# Data Aggregation and Group Operations

Categorizing a dataset and applying a function to each group, whether an aggregation or transformation, is often a critical component of a data analysis workflow. After loading, merging, and preparing a dataset, you may need to compute group statistics or possibly pivot tables for reporting or visualization purposes. pandas provides a flexible groupby interface, enabling you to slice, dice, and summarize datasets in a natural way.

## GroupBy Mechanics

Hadley Wickham, an author of many popular packages for the R programming language, coined the term split-apply-combine for describing group operations. In the first stage of the process, data contained in a pandas object, whether a Series, DataFrame, or otherwise, is split into groups based on one or more keys that you provide. The splitting is performed on a particular axis of an object. For example, a DataFrame can be grouped on its rows (axis=0) or its columns (axis=1). Once this is done, a function is applied to each group, producing a new value. Finally, the results of all those function applications are combined into a result object. To get started, here is a small tabular dataset as a DataFrame :

```python
In [10]: df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],
   ....:                    'key2' : ['one', 'two', 'one', 'two', 'one'],
   ....:                    'data1' : np.random.randn(5),
   ....:                    'data2' : np.random.randn(5)})
In [11]: df
```
> data1     data2 key1 key2<br>
> 0 -0.204708  1.393406    a  one<br>
> 1  0.478943  0.092908    a  two<br>
> 2 -0.519439  0.281746    b  one<br>
> 3 -0.555730  0.769023    b  two<br>
> 4  1.965781  1.246435    a  one<br>

Suppose you wanted to compute the mean of the data1 column using the labels from key1. There are a number of ways to do this. One is to access data1 and call groupby with the column (a Series) at key1 :

```python
In [12]: grouped = df['data1'].groupby(df['key1'])
In [13]: grouped
```
>  pandas.core.groupby.SeriesGroupBy object at 0x7faa31537390

This grouped variable is now a GroupBy object. It has not actually computed anything yet except for some intermediate data about the group key df['key1']. The idea is that this object has all of the information needed to then apply some operation to each of the groups. For example, to compute group means we can call the GroupBy’s mean method :

```python
In [14]: grouped.mean()
```
> key1<br>
> a    0.746672<br>
> b   -0.537585<br>
> Name: data1, dtype: float64<br>

If instead we had passed multiple arrays as a list, we’d get something different :

```pytohn
In [15]: means = df['data1'].groupby([df['key1'], df['key2']]).mean()
In [16]: means
```
> key1  key2<br>
> a     one     0.880536<br>
>      two     0.478943<br>
> b     one    -0.519439<br>
>      two    -0.555730<br>
> Name: data1, dtype: float64<br>

Here we grouped the data using two keys, and the resulting Series now has a hierarchical index consisting of the unique pairs of keys observed. Regardless of the objective in using groupby, a generally useful GroupBy method is size, which returns a Series containing group sizes :

```python
In [23]: df.groupby(['key1', 'key2']).size()
```
> key1  key2<br>
> a     one     2<br>
>      two     1<br>
> b     one     1<br>
>      two     1<br>
> dtype: int64<br>

### Iterating Over Groups

The GroupBy object supports iteration, generating a sequence of 2-tuples containing the group name along with the chunk of data. Consider the following :

```python
In [24]: for name, group in df.groupby('key1'):
   ....:     print(name)
   ....:     print(group)
   ....:
```
> a<br>
>      data1     data2 key1 key2<br>
> 0 -0.204708  1.393406    a  one<br>
> 1  0.478943  0.092908    a  two<br>
> 4  1.965781  1.246435    a  one<br>
> b<br>
>      data1     data2 key1 key2<br>
> 2 -0.519439  0.281746    b  one<br>
> 3 -0.555730  0.769023    b  two<br>

In the case of multiple keys, the first element in the tuple will be a tuple of key values :

```python
In [25]: for (k1, k2), group in df.groupby(['key1', 'key2']):
   ....:     print((k1, k2))
   ....:     print(group)
```

By default groupby groups on axis=0, but you can group on any of the other axes.

### Selecting a Column or Subset of Columns

Indexing a GroupBy object created from a DataFrame with a column name or array of column names has the effect of column subsetting for aggregation. This means that :

```python
df.groupby('key1')['data1']
df.groupby('key1')[['data2']]
```

are syntactic sugar for :

```python
df['data1'].groupby(df['key1'])
df[['data2']].groupby(df['key1'])
```

### Grouping with Dict and Series

Grouping information may exist in a form other than an array. Now, suppose we have a group correspondence for the columns and want to sum together the columns by group :

```python
In [38]: mapping = {'a': 'red', 'b': 'red', 'c': 'blue',
   ....:            'd': 'blue', 'e': 'red', 'f' : 'orange'}
```

Now, you could construct an array from this dict to pass to groupby, but instead we can just pass the dict. 

```python
In [39]: by_column = people.groupby(mapping, axis=1)
```

## Data Aggregation

Aggregations refer to any data transformation that produces scalar values from arrays. The preceding examples have used several of them, including mean, count, min, and sum. Optimized groupby methods :

| Function name        | Description          |
|:-------------|:------------------|
| count | Number of non-NA values in the group |
| sum | Sum of non-NA values |
| mean | Mean of non-NA values |
| median | Arithmetic median of non-NA values |
| std, var | Unbiased (n – 1 denominator) standard deviation and variance |
| min, max | Minimum and maximum of non-NA values |
| prod | Product of non-NA values |
| first, last | First and last non-NA values |

To use your own aggregation functions, pass any function that aggregates an array to the aggregate or agg method :

```python
In [54]: def peak_to_peak(arr):
   ....:     return arr.max() - arr.min()
In [55]: grouped.agg(peak_to_peak)
```
> data1     data2<br>
> key1<br>                    
> a     2.170488  1.300498<br>
> b     0.036292  0.487276<br>

### Column-Wise and Multiple Function Application
