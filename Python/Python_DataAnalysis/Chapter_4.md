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
