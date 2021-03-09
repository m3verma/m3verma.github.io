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

As you’ve already seen, aggregating a Series or all of the columns of a DataFrame is a matter of using aggregate with the desired function or calling a method like mean or std. However, you may want to aggregate using a different function depending on the column, or multiple functions at once. Fortunately, this is possible to do. you can pass the name of the function as a string :

```python
In [61]: grouped_pct = grouped['tip_pct']
In [62]: grouped_pct.agg('mean')
```
> day   smoker<br>
> Fri   No        0.151650<br>
>      Yes       0.174783<br>
> Sat   No        0.158048<br>
>      Yes       0.147906<br>
      
If you pass a list of functions or function names instead, you get back a DataFrame with column names taken from the functions :

```pytohn
In [63]: grouped_pct.agg(['mean', 'std', peak_to_peak])
```
> mean       std  peak_to_peak<br>
> day  smoker                     <br>             
> Fri  No      0.151650  0.028123      0.067349<br>
>     Yes     0.174783  0.051293      0.159925<br>
> Sat  No      0.158048  0.039767      0.235193<br>
>     Yes     0.147906  0.061375      0.290095<br>

With a DataFrame you have more options, as you can specify a list of functions to apply to all of the columns or different functions per column. Now, suppose you wanted to apply potentially different functions to one or more of the columns. To do this, pass a dict to agg that contains a mapping of column names to any of the function specifications listed so far :

```python
In [71]: grouped.agg({'tip' : np.max, 'size' : 'sum'})
```
> tip  size<br>
> day  smoker  <br>           
> Fri  No       3.50     9<br>
>     Yes      4.73    31<br>
> Sat  No       9.00   115<br>
>     Yes     10.00   104<br>

### Returning Aggregated Data Without Row Indexes

In all of the examples up until now, the aggregated data comes back with an index, potentially hierarchical, composed from the unique group key combinations. Since this isn’t always desirable, you can disable this behavior in most cases by passing as_index=False to groupby :

```python
In [73]: tips.groupby(['day', 'smoker'], as_index=False).mean()
```
> day smoker  total_bill       tip      size   tip_pct<br>
> 0   Fri     No   18.420000  2.812500  2.250000  0.151650<br>
> 1   Fri    Yes   16.813333  2.714000  2.066667  0.174783<br>
> 2   Sat     No   19.661778  3.102889  2.555556  0.158048<br>
> 3   Sat    Yes   21.276667  2.875476  2.476190  0.147906<br>

## Apply: General split-apply-combine

apply splits the object being manipulated into pieces, invokes the passed function on each piece, and then attempts to concatenate the pieces together. Returning to the tipping dataset from before, suppose you wanted to select the top five tip_pct values by group. First, write a function that selects the rows with the largest values in a particular column :

```python
In [74]: def top(df, n=5, column='tip_pct'):
   ....:     return df.sort_values(by=column)[-n:]
In [75]: top(tips, n=6)
```
> total_bill   tip smoker  day    time  size   tip_pct<br>
> 109       14.31  4.00    Yes  Sat  Dinner     2  0.279525<br>
> 183       23.17  6.50    Yes  Sun  Dinner     4  0.280535<br>
> 232       11.61  3.39     No  Sat  Dinner     2  0.291990<br>
> 67         3.07  1.00    Yes  Sat  Dinner     1  0.325733<br>
> 178        9.60  4.00    Yes  Sun  Dinner     2  0.416667<br>
> 172        7.25  5.15    Yes  Sun  Dinner     2  0.710345<br>

Now, if we group by smoker, say, and call apply with this function, we get the following :

```python
In [76]: tips.groupby('smoker').apply(top)
```
> total_bill   tip smoker   day    time  size   tip_pct<br>
> smoker                                                   <br>        
> No     88        24.71  5.85     No  Thur   Lunch     2  0.236746<br>
>       185       20.69  5.00     No   Sun  Dinner     5  0.241663<br>
>       51        10.29  2.60     No   Sun  Dinner     2  0.252672<br>
>       149        7.51  2.00     No  Thur   Lunch     2  0.266312<br>
>       232       11.61  3.39     No   Sat  Dinner     2  0.291990<br>

What has happened here? The top function is called on each row group from the DataFrame, and then the results are glued together using pandas.concat, labeling the pieces with the group names. The result therefore has a hierarchical index whose inner level contains index values from the original DataFrame.

### Quantile and Bucket Analysis

pandas has some tools, in particular cut and qcut, for slicing data up into buckets with bins of your choosing or by sample quantiles. Combining these functions with groupby makes it convenient to perform bucket or quantile analysis on a dataset. Consider a simple random dataset and an equal-length bucket categorization using cut :

```python
In [82]: frame = pd.DataFrame({'data1': np.random.randn(1000),
   ....:                       'data2': np.random.randn(1000)})
In [83]: quartiles = pd.cut(frame.data1, 4)
```

The Categorical object returned by cut can be passed directly to groupby. So we could compute a set of statistics for the data2 column like so.

## Pivot Tables and Cross-Tabulation

A pivot table is a data summarization tool frequently found in spreadsheet programs and other data analysis software. It aggregates a table of data by one or more keys, arranging the data in a rectangle with some of the group keys along the rows and some along the columns. Pivot tables in Python with pandas are made possible through the groupby facility. DataFrame has a pivot_table method, and there is also a top-level pandas.pivot_table function. In addition to providing a convenience interface to groupby, pivot_table can add partial totals, also known as margins. Returning to the tipping dataset, suppose you wanted to compute a table of group means (the default pivot_table aggregation type) arranged by day and smoker on the rows :

```python
In [130]: tips.pivot_table(index=['day', 'smoker'])
``` 
> size       tip   tip_pct  total_bill<br>
> day  smoker                             <br>             
>Fri  No      2.250000  2.812500  0.151650   18.420000<br>
>     Yes     2.066667  2.714000  0.174783   16.813333<br>
>Sat  No      2.555556  3.102889  0.158048   19.661778<br>
>     Yes     2.476190  2.875476  0.147906   21.276667<br>
>Sun  No      2.929825  3.167895  0.160113   20.506667<br>
>     Yes     2.578947  3.516842  0.187250   24.120000<br>
>Thur No      2.488889  2.673778  0.160298   17.113111<br>
>     Yes     2.352941  3.030000  0.163863   19.190588<br>

We could augment this table to include partial totals by passing margins=True. This has the effect of adding All row and column labels, with corresponding values being the group statistics for all the data within a single tier :

```python
In [132]: tips.pivot_table(['tip_pct', 'size'], index=['time', 'day'],
   .....:                  columns='smoker', margins=True)
``` 
> size                       tip_pct                    <br>
> smoker             No       Yes       All        No       Yes       All<br>
> time   day                                                             <br>
> Dinner Fri   2.000000  2.222222  2.166667  0.139622  0.165347  0.158916<br>
>       Sat   2.555556  2.476190  2.517241  0.158048  0.147906  0.153152<br>
>       Sun   2.929825  2.578947  2.842105  0.160113  0.187250  0.166897<br>
>       Thur  2.000000       NaN  2.000000  0.159744       NaN  0.159744<br>
> Lunch  Fri   3.000000  1.833333  2.000000  0.187735  0.188937  0.188765<br>
>       Thur  2.500000  2.352941  2.459016  0.160311  0.163863  0.161301<br>
> All          2.668874  2.408602  2.569672  0.159328  0.163196  0.160803<br>

Pivot_table options :

| Function name        | Description          |
|:-------------|:------------------|
| values | Column name or names to aggregate; by default aggregates all numeric columns |
| index | Column names or other group keys to group on the rows of the resulting pivot table |
| columns | Column names or other group keys to group on the columns of the resulting pivot table |
| aggfunc | Aggregation function or list of functions ('mean' by default); can be any function valid in a groupby context |
| fill_value | Replace missing values in result table |
| dropna | If True, do not include columns whose entries are all NA |
| margins | Add row/column subtotals and grand total (False by default) |

### Cross-Tabulations: Crosstab

A cross-tabulation (or crosstab for short) is a special case of a pivot table that computes group frequencies. Here is an example :

```python
In [138]: data
``` 
> Sample Nationality    Handedness<br>
> 0       1         USA  Right-handed<br>
> 1       2       Japan   Left-handed<br>
> 2       3         USA  Right-handed<br>
> 3       4       Japan  Right-handed<br>
> 4       5       Japan   Left-handed<br>
> 5       6       Japan  Right-handed<br>
> 6       7         USA  Right-handed<br>
> 7       8         USA   Left-handed<br>
> 8       9       Japan  Right-handed<br>
> 9      10         USA  Right-handed<br>

As part of some survey analysis, we might want to summarize this data by nationality and handedness. You could use pivot_table to do this, but the pandas.crosstab function can be more convenient :

```python
In [139]: pd.crosstab(data.Nationality, data.Handedness, margins=True)
``` 
> Handedness   Left-handed  Right-handed  All<br>
> Nationality                                <br>
> Japan                  2             3    5<br>
> USA                    1             4    5<br>
> All                    3             7   10<br>
