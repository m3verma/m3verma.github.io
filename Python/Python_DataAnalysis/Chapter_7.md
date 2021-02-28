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


