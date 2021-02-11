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
