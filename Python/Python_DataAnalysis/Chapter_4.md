---
layout: default
---

# NumPy Basics: Arrays and Vectorized Computation

## NumPy

NumPy, short for Numerical Python, is one of the most important foundational packages for numerical computing in Python. Most computational packages providing scientific functionality use NumPy’s array objects as the lingua franca for data exchange. Here are some of the things you’ll find in NumPy :

1. ndarray, an efficient multidimensional array providing fast array-oriented arithmetic operations and flexible broadcasting capabilities.
2. Mathematical functions for fast operations on entire arrays of data without having to write loops.
3. Tools for reading/writing array data to disk and working with memory-mapped files.
4. Linear algebra, random number generation, and Fourier transform capabilities.
5. A C API for connecting NumPy with libraries written in C, C++, or FORTRAN.

One of the reasons NumPy is so important for numerical computations in Python is because it is designed for efficiency on large arrays of data. There are a number of reasons for this :

1. NumPy internally stores data in a contiguous block of memory, independent of other built-in Python objects. NumPy’s library of algorithms written in the C language can operate on this memory without any type checking or other overhead. NumPy arrays also use much less memory than built-in Python sequences.
2. NumPy operations perform complex computations on entire arrays without the need for Python for loops.

To give you an idea of the performance difference, consider a NumPy array of one million integers, and the equivalent Python list :

```python
In [7]: import numpy as np
In [8]: my_arr = np.arange(1000000)
In [9]: my_list = list(range(1000000))
```

Now let’s multiply each sequence by 2 :

```python
In [10]: %time for _ in range(10): my_arr2 = my_arr * 2
```
> CPU times: user 20 ms, sys: 50 ms, total: 70 ms
> Wall time: 72.4 ms

```python
In [11]: %time for _ in range(10): my_list2 = [x * 2 for x in my_list]
```
> CPU times: user 760 ms, sys: 290 ms, total: 1.05 s
> Wall time: 1.05 s

## The NumPy ndarray: A Multidimensional Array Object

One of the key features of NumPy is its N-dimensional array object, or ndarray, which is a fast, flexible container for large datasets in Python.

```python
In [12]: import numpy as np
# Generate some random data
In [13]: data = np.random.randn(2, 3)
In [14]: data
```
> array([[-0.2047,  0.4789, -0.5194],[-0.5557,  1.9658,  1.3934]])

```python
In [15]: data * 10
```
> array([[ -2.0471,   4.7894,  -5.1944],[ -5.5573,  19.6578,  13.9341]])

An ndarray is a generic multidimensional container for homogeneous data; that is, all of the elements must be the same type. Every array has a shape, a tuple indicating the size of each dimension, and a dtype, an object describing the data type of the array :

```python
In [17]: data.shape
```
> (2, 3)

```python
In [18]: data.dtype
```
> dtype('float64')

### Creating ndarrays

The easiest way to create an array is to use the array function. This accepts any sequence-like object (including other arrays) and produces a new NumPy array containing the passed data.

```python
In [19]: data1 = [6, 7.5, 8, 0, 1]
In [20]: arr1 = np.array(data1)
In [21]: arr1
```
> array([ 6. ,  7.5,  8. ,  0. ,  1. ])

Nested sequences, like a list of equal-length lists, will be converted into a multidimensional array :

```python
In [22]: data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
In [23]: arr2 = np.array(data2)
In [24]: arr2
```
> array([[1, 2, 3, 4],[5, 6, 7, 8]])

Since data2 was a list of lists, the NumPy array arr2 has two dimensions with shape inferred from the data. We can confirm this by inspecting the ndim and shape attributes :

```python
In [25]: arr2.ndim
```
> 2

```python
In [26]: arr2.shape
```
> (2, 4)

In addition to np.array, there are a number of other functions for creating new arrays. As examples, zeros and ones create arrays of 0s or 1s, respectively, with a given length or shape. empty creates an array without initializing its values to any particular value. To create a higher dimensional array with these methods, pass a tuple for the shape :

```python
In [29]: np.zeros(10)
```
> array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])

```python
In [30]: np.zeros((3, 6))
```
> array([[ 0.,  0.,  0.,  0.,  0.,  0.],[ 0.,  0.,  0.,  0.,  0.,  0.],[ 0.,  0.,  0.,  0.,  0.,  0.]])

arange is an array-valued version of the built-in Python range function :

```python
In [32]: np.arange(15)
```
> array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])

Array creation functions :

| Function        | Description          |
|:-------------|:------------------|
| array | Convert input data (list, tuple, array, or other sequence type) to an ndarray either by inferring a dtype or explicitly specifying a dtype; copies the input data by default |
| asarray | Convert input to ndarray, but do not copy if the input is already an ndarray |
| arange | Like the built-in range but returns an ndarray instead of a list |
| ones, ones_like | Produce an array of all 1s with the given shape and dtype; ones_like takes another array and produces a ones array of the same shape and dtype |
| zeros, zeros_like | Like ones and ones_like but producing arrays of 0s instead |
| empty, empty_like | Create new arrays by allocating new memory, but do not populate with any values like ones and zeros |
| full, full_like | Produce an array of the given shape and dtype with all values set to the indicated “fill value” full_like takes another array and produces a filled array of the same shape and dtype |
| eye, identity | Create a square N × N identity matrix (1s on the diagonal and 0s elsewhere) |

### Data Types for ndarrays

The data type or dtype is a special object containing the information (or metadata, data about data) the ndarray needs to interpret a chunk of memory as a particular type of data :

```python
In [33]: arr1 = np.array([1, 2, 3], dtype=np.float64)
In [34]: arr2 = np.array([1, 2, 3], dtype=np.int32)
In [35]: arr1.dtype
```
> dtype('float64')

dtypes are a source of NumPy’s flexibility for interacting with data coming from other systems. In most cases they provide a mapping directly onto an underlying disk or memory representation, which makes it easy to read and write binary streams of data to disk and also to connect to code written in a low-level language like C or Fortran. You can explicitly convert or cast an array from one dtype to another using ndarray’s astype method :

```python
In [37]: arr = np.array([1, 2, 3, 4, 5])
In [39]: float_arr = arr.astype(np.float64)
In [40]: float_arr.dtype
```
> dtype('float64')

### Arithmetic with NumPy Arrays

Arrays are important because they enable you to express batch operations on data without writing any for loops. NumPy users call this vectorization. Any arithmetic operations between equal-size arrays applies the operation element-wise :

```python
In [51]: arr = np.array([[1., 2., 3.], [4., 5., 6.]])
In [53]: arr * arr
```
> array([[  1.,   4.,   9.],[ 16.,  25.,  36.]])

Arithmetic operations with scalars propagate the scalar argument to each element in the array :

```python
In [55]: 1 / arr
```
> array([[ 1.    ,  0.5   ,  0.3333],[ 0.25  ,  0.2   ,  0.1667]])

Comparisons between arrays of the same size yield boolean arrays :

```python
In [57]: arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])
In [59]: arr2 > arr
```
> array([[False,  True, False],[ True, False,  True]], dtype=bool)

### Basic Indexing and Slicing

NumPy array indexing is a rich topic, as there are many ways you may want to select a subset of your data or individual elements. One-dimensional arrays are simple; on the surface they act similarly to Python lists :

```python
In [60]: arr = np.arange(10)
In [61]: arr
In [62]: arr[5]
```
> 5

```python
In [63]: arr[5:8]
```
> array([5, 6, 7])

```python
In [64]: arr[5:8] = 12
In [65]: arr
```
> array([ 0,  1,  2,  3,  4, 12, 12, 12,  8,  9])

As you can see, if you assign a scalar value to a slice, as in arr[5:8] = 12, the value is propagated (or broadcasted henceforth) to the entire selection. An important first distinction from Python’s built-in lists is that array slices are views on the original array. This means that the data is not copied, and any modifications to the view will be reflected in the source array. With higher dimensional arrays, you have many more options. In a two-dimensional array, the elements at each index are no longer scalars but rather one-dimensional arrays :

```python
In [72]: arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
In [73]: arr2d[2]
```
> array([7, 8, 9])

Thus, individual elements can be accessed recursively. But that is a bit too much work, so you can pass a comma-separated list of indices to select individual elements. So these are equivalent :

```python
In [74]: arr2d[0][2]
```
> 3

```python
In [75]: arr2d[0, 2]
```
> 3

In multidimensional arrays, if you omit later indices, the returned object will be a lower dimensional ndarray consisting of all the data along the higher dimensions. So in the 2 × 2 × 3 array arr3d :

```python
In [76]: arr3d = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
```
> array([[[ 1,  2,  3],[ 4,  5,  6]],[[ 7,  8,  9],[10, 11, 12]]])

arr3d[0] is a 2 × 3 array :

```python
In [78]: arr3d[0]
```
> array([[1, 2, 3],[4, 5, 6]])

Like one-dimensional objects such as Python lists, ndarrays can be sliced with the familiar syntax. Consider the two-dimensional array from before, arr2d. Slicing this array is a bit different :

```python
In [91]: arr2d[:2]
```
> array([[1, 2, 3],[4, 5, 6]])

You can pass multiple slices just like you can pass multiple indexes :

```python
In [92]: arr2d[:2, 1:]
```
> array([[2, 3],[5, 6]])

When slicing like this, you always obtain array views of the same number of dimensions. By mixing integer indexes and slices, you get lower dimensional slices.

### Boolean Indexing

Let’s consider an example where we have some data in an array and an array of names with duplicates.

```python
In [98]: names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
In [99]: data = np.random.randn(7, 4)
In [101]: data
```
> array([[ 0.0929,  0.2817,  0.769 ,  1.2464],[ 1.0072, -1.2962,  0.275 ,  0.2289],[ 1.3529,  0.8864, -2.0016, -0.3718],[ 1.669 , -0.4386, -0.5397,  0.477 ],[ 3.2489, -1.0212, -0.5771,  0.1241],[ 0.3026,  0.5238,  0.0009,  1.3438],[-0.7135, -0.8312, -2.3702, -1.8608]])

Suppose each name corresponds to a row in the data array and we wanted to select all the rows with corresponding name 'Bob'. Like arithmetic operations, comparisons (such as ==) with arrays are also vectorized. Thus, comparing names with the string 'Bob' yields a boolean array :

```python
In [102]: names == 'Bob'
```
> array([ True, False, False,  True, False, False, False], dtype=bool)

This boolean array can be passed when indexing the array :

```python
In [103]: data[names == 'Bob']
```
> array([[ 0.0929,  0.2817,  0.769 ,  1.2464],[ 1.669 , -0.4386, -0.5397,  0.477 ]])

In these examples, I select from the rows where names == 'Bob' and index the columns, too :

```python
In [104]: data[names == 'Bob', 2:]
```
> array([[ 0.769 ,  1.2464],[-0.5397,  0.477 ]])

Selecting two of the three names to combine multiple boolean conditions, use boolean arithmetic operators like & (and) and | (or) :

```python
In [110]: mask = (names == 'Bob') | (names == 'Will')
In [111]: mask
```
> array([ True, False,  True,  True,  True, False, False], dtype=bool)

Setting values with boolean arrays works in a common-sense way. To set all of the negative values in data to 0 we need only do :

```python
In [113]: data[data < 0] = 0
In [114]: data
```
> array([[ 0.0929,  0.2817,  0.769 ,  1.2464],[ 1.0072,  0.    ,  0.275 ,  0.2289],[ 1.3529,  0.8864,  0.    ,  0.    ],[ 1.669 ,  0.    ,  0.    ,  0.477 ],[ 3.2489,  0.    ,  0.    ,  0.1241],[ 0.3026,  0.5238,  0.0009,  1.3438],[ 0.    ,  0.    ,  0.    ,  0.    ]])

### Fancy Indexing

Fancy indexing is a term adopted by NumPy to describe indexing using integer arrays. Suppose we had an 8 × 4 array :

```python
In [117]: arr = np.empty((8, 4))
In [118]: for i in range(8):
   .....:     arr[i] = i
In [119]: arr
```
> array([[ 0.,  0.,  0.,  0.],[ 1.,  1.,  1.,  1.],[ 2.,  2.,  2.,  2.],[ 3.,  3.,  3.,  3.],[ 4.,  4.,  4.,  4.],[ 5.,  5.,  5.,  5.],[ 6.,  6.,  6.,  6.],[ 7.,  7.,  7.,  7.]])

To select out a subset of the rows in a particular order, you can simply pass a list or ndarray of integers specifying the desired order :

```python
In [120]: arr[[4, 3, 0, 6]]
```
> array([[ 4.,  4.,  4.,  4.],[ 3.,  3.,  3.,  3.],[ 0.,  0.,  0.,  0.],[ 6.,  6.,  6.,  6.]])

Passing multiple index arrays does something slightly different; it selects a one-dimensional array of elements corresponding to each tuple of indices :

```python
In [122]: arr = np.arange(32).reshape((8, 4))
In [123]: arr
```
> array([[ 0,  1,  2,  3],[ 4,  5,  6,  7],[ 8,  9, 10, 11],[12, 13, 14, 15],[16, 17, 18, 19],[20, 21, 22, 23],[24, 25, 26, 27],[28, 29, 30, 31]])

```python
In [124]: arr[[1, 5, 7, 2], [0, 3, 1, 2]]
```
> array([ 4, 23, 29, 10])

### Transposing Arrays and Swapping Axes

Transposing is a special form of reshaping that similarly returns a view on the underlying data without copying anything. Arrays have the transpose method and also the special T attribute :

```python
In [126]: arr = np.arange(15).reshape((3, 5))
In [127]: arr
```
> array([[ 0,  1,  2,  3,  4],[ 5,  6,  7,  8,  9],[10, 11, 12, 13, 14]])

```python
In [128]: arr.T
```
> array([[ 0,  5, 10],[ 1,  6, 11],[ 2,  7, 12],[ 3,  8, 13],[ 4,  9, 14]])

When doing matrix computations, you may do this very often—for example, when computing the inner matrix product using np.dot :

```python
In [129]: arr = np.random.randn(6, 3)
In [130]: arr
```
> array([[-0.8608,  0.5601, -1.2659],[ 0.1198, -1.0635,  0.3329],[-2.3594, -0.1995, -1.542 ],[-0.9707, -1.307 ,  0.2863],[ 0.378 , -0.7539,  0.3313],[ 1.3497,  0.0699,  0.2467]])

```python
In [131]: np.dot(arr.T, arr)
``` 
> array([[ 9.2291,  0.9394,  4.948 ],[ 0.9394,  3.7662, -1.3622],[ 4.948 , -1.3622,  4.3437]])

## Universal Functions: Fast Element-Wise Array Functions

A universal function, or ufunc, is a function that performs element-wise operations on data in ndarrays. You can think of them as fast vectorized wrappers for simple functions that take one or more scalar values and produce one or more scalar results. Many ufuncs are simple element-wise transformations, like sqrt or exp :

```python
In [137]: arr = np.arange(10)
In [138]: arr
```
> array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

```python
In [139]: np.sqrt(arr) 
```
> array([ 0.    ,  1.    ,  1.4142,  1.7321,  2.    ,  2.2361,  2.4495, 2.6458,  2.8284,  3.    ])

These are referred to as unary ufuncs. Others, such as add or maximum, take two arrays (thus, binary ufuncs) and return a single array as the result :

```python
In [141]: x = np.random.randn(8)
In [142]: y = np.random.randn(8)
In [143]: x
```
> array([-0.0119,  1.0048,  1.3272, -0.9193, -1.5491,  0.0222,  0.7584, -0.6605])

```python
In [144]: y
``` 
> array([ 0.8626, -0.01  ,  0.05  ,  0.6702,  0.853 , -0.9559, -0.0235, -2.3042])

```python
In [145]: np.maximum(x, y)
``` 
> array([ 0.8626,  1.0048,  1.3272,  0.6702,  0.853 ,  0.0222,  0.7584, -0.6605])

Here, numpy.maximum computed the element-wise maximum of the elements in x and y. While not common, a ufunc can return multiple arrays. modf is one example, a vectorized version of the built-in Python divmod; it returns the fractional and integral parts of a floating-point array :

```python
In [146]: arr = np.random.randn(7) * 5
In [147]: arr
```
> array([-3.2623, -6.0915, -6.663 ,  5.3731,  3.6182,  3.45  ,  5.0077])

```python
In [148]: remainder, whole_part = np.modf(arr)
In [149]: remainder
```
> array([-0.2623, -0.0915, -0.663 ,  0.3731,  0.6182,  0.45  ,  0.0077])

```python
In [150]: whole_part
```
> array([-3., -6., -6.,  5.,  3.,  3.,  5.])

Unary ufuncs :

| Function        | Description          |
|:-------------|:------------------|
| abs, fabs | Compute the absolute value element-wise for integer, floating-point, or complex values |
| sqrt | Compute the square root of each element (equivalent to arr ** 0.5) |
| square | Compute the square of each element (equivalent to arr ** 2) |
| exp | Compute the exponent ex of each element |
| log, log10, log2, log1p | Natural logarithm (base e), log base 10, log base 2, and log(1 + x), respectively |
| sign | Compute the sign of each element: 1 (positive), 0 (zero), or –1 (negative) |
| ceil | Compute the ceiling of each element (i.e., the smallest integer greater than or equal to that number) |
| floor | Compute the floor of each element (i.e., the largest integer less than or equal to each element) |
| rint | Round elements to the nearest integer, preserving the dtype |
| modf | Return fractional and integral parts of array as a separate array |
| isnan | Return boolean array indicating whether each value is NaN (Not a Number) |
| isfinite, isinf | Return boolean array indicating whether each element is finite (non-inf, non-NaN) or infinite, respectively |
| cos, cosh, sin, sinh, tan, tanh | Regular and hyperbolic trigonometric functions |
| arccos, arccosh, arcsin, arcsinh, arctan, arctanh | Inverse trigonometric functions |
| logical_not | Compute truth value of not x element-wise (equivalent to ~arr). |

Binary universal functions :

| Function        | Description          |
|:-------------|:------------------|
| add | Add corresponding elements in arrays |
| subtract | Subtract elements in second array from first array |
| multiply | Multiply array elements |
| divide, floor_divide | Divide or floor divide (truncating the remainder) |
| power | Raise elements in first array to powers indicated in second array |
| maximum, fmax | Element-wise maximum; fmax ignores NaN |
| minimum, fmin | Element-wise minimum; fmin ignores NaN |
| mod | Element-wise modulus (remainder of division) |
| copysign | Copy sign of values in second argument to values in first argument |
| greater, greater_equal, less, less_equal, equal, not_equal | Perform element-wise comparison, yielding boolean array (equivalent to infix operators >, >=, <, <=, ==, !=) |
| logical_and, logical_or, logical_xor | Compute element-wise truth value of logical operation (equivalent to infix operators & |, ^) |

## Array-Oriented Programming with Arrays

Using NumPy arrays enables you to express many kinds of data processing tasks as concise array expressions that might otherwise require writing loops. This practice of replacing explicit loops with array expressions is commonly referred to as vectorization. In general, vectorized array operations will often be one or two (or more) orders of magnitude faster than their pure Python equivalents, with the biggest impact in any kind of numerical computations. The np.meshgrid function takes two 1D arrays and produces two 2D matrices corresponding to all pairs of (x, y) in the two arrays :

```python
In [155]: points = np.arange(-5, 5, 0.01) # 1000 equally spaced points
In [156]: xs, ys = np.meshgrid(points, points)
In [157]: ys
```
> array([[-5.  , -5.  , -5.  , ..., -5.  , -5.  , -5.  ],[-4.99, -4.99, -4.99, ..., -4.99, -4.99, -4.99],[-4.98, -4.98, -4.98, ..., -4.98, -4.98, -4.98],..., [ 4.97,  4.97,  4.97, ...,  4.97,  4.97,  4.97],[ 4.98,  4.98,  4.98, ...,  4.98,  4.98,  4.98],[ 4.99,  4.99,  4.99, ...,  4.99,  4.99,  4.99]])

### Expressing Conditional Logic as Array Operations

The numpy.where function is a vectorized version of the ternary expression x if condition else y. Suppose we had a boolean array and two arrays of values :

```python
In [165]: xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
In [166]: yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
In [167]: cond = np.array([True, False, True, True, False])
```

Suppose we wanted to take a value from xarr whenever the corresponding value in cond is True, and otherwise take the value from yarr.

```python
In [170]: result = np.where(cond, xarr, yarr)
In [171]: result
```
> array([ 1.1,  2.2,  1.3,  1.4,  2.5])

You can combine scalars and arrays when using np.where. For example, I can replace all positive values in arr with the constant 2 like so :

```python
In [176]: np.where(arr > 0, 2, arr) # set only positive values to 2
```
> array([[-0.5031, -0.6223, -0.9212, -0.7262],[ 2.    ,  2.    , -1.1577,  2.    ],[ 2.    ,  2.    ,  2.    , -0.9975],[ 2.    , -0.1316,  2.    ,  2.    ]])

### Mathematical and Statistical Methods

A set of mathematical functions that compute statistics about an entire array or about the data along an axis are accessible as methods of the array class. You can use aggregations (often called reductions) like sum, mean, and std (standard deviation) either by calling the array instance method or using the top-level NumPy function.

```python
In [177]: arr = np.random.randn(5, 4)
In [178]: arr
```
> array([[ 2.1695, -0.1149,  2.0037,  0.0296],[ 0.7953,  0.1181, -0.7485,  0.585 ],[ 0.1527, -1.5657, -0.5625, -0.0327],[-0.929 , -0.4826, -0.0363,  1.0954],[ 0.9809, -0.5895,  1.5817, -0.5287]])

```python
In [179]: arr.mean()
```
> 0.19607051119998253

```python
In [180]: np.mean(arr)
```
> 0.19607051119998253

```python
In [181]: arr.sum()
```
> 3.9214102239996507

Other methods like cumsum and cumprod do not aggregate, instead producing an array of the intermediate results :

```python
In [184]: arr = np.array([0, 1, 2, 3, 4, 5, 6, 7])
In [185]: arr.cumsum()
```
> array([ 0,  1,  3,  6, 10, 15, 21, 28])

Basic Array Statistical methods :

| Function        | Description          |
|:-------------|:------------------|
| sum | Sum of all the elements in the array or along an axis; zero-length arrays have sum 0 |
| mean | Arithmetic mean; zero-length arrays have NaN mean |
| std, var | Standard deviation and variance, respectively, with optional degrees of freedom adjustment (default denominator n) |
| min, max | Minimum and maximum |
| argmin, argmax | Indices of minimum and maximum elements, respectively |
| cumsum | Cumulative sum of elements starting from 0 |
| cumprod | Cumulative product of elements starting from 1 |

### Sorting

Like Python’s built-in list type, NumPy arrays can be sorted in-place with the sort method :

```python
In [195]: arr = np.random.randn(6)
In [196]: arr
```
> array([ 0.6095, -0.4938,  1.24  , -0.1357,  1.43  , -0.8469])

```python
In [197]: arr.sort()
In [198]: arr
```
> array([-0.8469, -0.4938, -0.1357,  0.6095,  1.24  ,  1.43  ])

### Unique and Other Set Logic

NumPy has some basic set operations for one-dimensional ndarrays. A commonly used one is np.unique, which returns the sorted unique values in an array :

```python
In [206]: names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
In [207]: np.unique(names)
```
> array(['Bob', 'Joe', 'Will'], dtype='<U4')

Another function, np.in1d, tests membership of the values in one array in another, returning a boolean array :

```python
In [211]: values = np.array([6, 0, 0, 3, 2, 5, 6])
In [212]: np.in1d(values, [2, 3, 6])
```
> array([ True, False, False,  True,  True, False,  True], dtype=bool)

Array set operations :

| Function        | Description          |
|:-------------|:------------------|
| unique(x) | Compute the sorted, unique elements in x |
| intersect1d(x, y) | Compute the sorted, common elements in x and y |
| union1d(x, y) | Compute the sorted union of elements |
| in1d(x, y) | Compute a boolean array indicating whether each element of x is contained in y |
| setdiff1d(x, y) | Set difference, elements in x that are not in y |
| setxor1d(x, y) | Set symmetric differences; elements that are in either of the arrays, but not both |

## File Input and Output with Arrays

NumPy is able to save and load data to and from disk either in text or binary format. np.save and np.load are the two workhorse functions for efficiently saving and loading array data on disk. Arrays are saved by default in an uncompressed raw binary format with file extension .npy :

```python
In [213]: arr = np.arange(10)
In [214]: np.save('some_array', arr)
```

If the file path does not already end in .npy, the extension will be appended. The array on disk can then be loaded with np.load :

```python
In [215]: np.load('some_array.npy')
```
> array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

You save multiple arrays in an uncompressed archive using np.savez and passing the arrays as keyword arguments :

```python
In [216]: np.savez('array_archive.npz', a=arr, b=arr)
```

When loading an .npz file, you get back a dict-like object that loads the individual arrays lazily :

```python
In [217]: arch = np.load('array_archive.npz')
In [218]: arch['b']
```
> array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

## Linear Algebra

Linear algebra, like matrix multiplication, decompositions, determinants, and other square matrix math, is an important part of any array library. Unlike some languages like MATLAB, multiplying two two-dimensional arrays with * is an element-wise product instead of a matrix dot product. Thus, there is a function dot, both an array method and a function in the numpy namespace, for matrix multiplication :

```python
In [223]: x = np.array([[1., 2., 3.], [4., 5., 6.]])
In [224]: y = np.array([[6., 23.], [-1, 7], [8, 9]])
In [227]: x.dot(y)
```
> array([[  28.,   64.], [  67.,  181.]])

The @ symbol (as of Python 3.5) also works as an infix operator that performs matrix multiplication :

```python
In [230]: x @ np.ones(3)
```
> array([  6.,  15.])

A list of some of the most commonly used linear algebra functions.

| Function        | Description          |
|:-------------|:------------------|
| diag | Return the diagonal (or off-diagonal) elements of a square matrix as a 1D array, or convert a 1D array into a square matrix with zeros on the off-diagonal |
| dot | Matrix multiplication |
| trace | Compute the sum of the diagonal elements |
| det | Compute the matrix determinant |
| eig | Compute the eigenvalues and eigenvectors of a square matrix |
| inv | Compute the inverse of a square matrix |
| pinv | Compute the Moore-Penrose pseudo-inverse of a matrix |
| qr | Compute the QR decomposition |
| svd | Compute the singular value decomposition (SVD) |
| solve | Solve the linear system Ax = b for x, where A is a square matrix |
| lstsq | Compute the least-squares solution to Ax = b |

## Pseudorandom Number Generation

The numpy.random module supplements the built-in Python random with functions for efficiently generating whole arrays of sample values from many kinds of probability distributions. For example, you can get a 4 × 4 array of samples from the standard normal distribution using normal :

```python
In [238]: samples = np.random.normal(size=(4, 4))
In [239]: samples
```
> array([[ 0.5732,  0.1933,  0.4429,  1.2796],[ 0.575 ,  0.4339, -0.7658, -1.237 ], [-0.5367,  1.8545, -0.92  , -0.1082], [ 0.1525,  0.9435, -1.0953, -0.144 ]])

We say that these are pseudorandom numbers because they are generated by an algorithm with deterministic behavior based on the seed of the random number generator. You can change NumPy’s random number generation seed using np.random.seed :

```python
In [244]: np.random.seed(1234)
```

Partial list of numpy.random functions :

| Function        | Description          |
|:-------------|:------------------|
| seed | Seed the random number generator |
| permutation | Return a random permutation of a sequence, or return a permuted range |
| shuffle | Randomly permute a sequence in-place |
| rand | Draw samples from a uniform distribution |
| randint | Draw random integers from a given low-to-high range |
| randn | Draw samples from a normal distribution with mean 0 and standard deviation 1 (MATLAB-like interface) |
| binomial | Draw samples from a binomial distribution |
| normal | Draw samples from a normal (Gaussian) distribution |
| beta | Draw samples from a beta distribution |
| chisquare | Draw samples from a chi-square distribution |
| gamma | Draw samples from a gamma distribution |
| uniform | Draw samples from a uniform [0, 1) distribution |

## Example : Random Walks

The simulation of random walks provides an illustrative application of utilizing array operations. Let’s first consider a simple random walk starting at 0 with steps of 1 and –1 occurring with equal probability. You might make the observation that walk is simply the cumulative sum of the random steps and could be evaluated as an array expression. Thus, I use the np.random module to draw 1,000 coin flips at once, set these to 1 and –1, and compute the cumulative sum :

```python
In [251]: nsteps = 1000
In [252]: draws = np.random.randint(0, 2, size=nsteps)
In [253]: steps = np.where(draws > 0, 1, -1)
In [254]: walk = steps.cumsum()
```

If your goal was to simulate many random walks, say 5,000 of them, you can generate all of the random walks with minor modifications to the preceding code. If passed a 2-tuple, the numpy.random functions will generate a two-dimensional array of draws, and we can compute the cumulative sum across the rows to compute all 5,000 random walks in one shot :

```python
In [258]: nwalks = 5000
In [259]: nsteps = 1000
In [260]: draws = np.random.randint(0, 2, size=(nwalks, nsteps)) # 0 or 1
In [261]: steps = np.where(draws > 0, 1, -1)
In [262]: walks = steps.cumsum(1)
```
