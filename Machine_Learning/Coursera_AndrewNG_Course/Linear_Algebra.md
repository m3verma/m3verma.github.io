---
layout: default
---


 <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 

# Linear Algebra

Linear algebra is a branch of mathematics, but the truth of it is that linear algebra is the mathematics of data. Matrices and vectors are the language of data. Linear algebra is about linear combinations. That is, using arithmetic on columns of numbers called vectors and arrays of numbers called matrices, to create new columns and arrays of numbers. Linear algebra is the study of lines and planes, vector spaces and mappings that are required for linear transforms.

## Matrices

In mathematics, a matrix (plural matrices) is a rectangular array or table of numbers, symbols, or expressions, arranged in rows and columns. In simpler terms these are 2-dimensional arrays. Here are 2 examples of a matrix A and B :
> A <br>
> 1 2 3 <br>
> 4 5 6 <br>
> 7 8 9 <br>
> B <br>
> 1 5 <br>
> 2 6 <br>
> 3 7 <br>
> 4 8 <br>

### Dimension of a Matrix

The size of a matrix is defined by the number of rows and columns that it contains. A matrix with m rows and n columns is called an m × n matrix, or m-by-n matrix, while m and n are called its dimensions. For example, the matrix A above is a 3 × 3 matrix and B is a 4 × 2 matrix.

## Vectors

Vectors are special matrix with 1 column and many rows. So, vectors are subsets of matrices and the dimension is always n × 1 .
> y <br>
> 1 <br>
> 4 <br>
> 7 <br>

So the dimension of the above vector is 3 × 1 .

## Some Notations

1. A<sub>ij</sub> = It refers to the element in the i<sup>th</sup> row and j<sup>th</sup> column of matrix A. So A<sub>12</sub> = 2.
2. A vector with 'n' rows is referred as n-dimensional vector.
3. y<sub>k</sub> = It refers to the element at k<sup>th</sup> row of vector.
4. In general, all matrix and vector are 1-indexed.
5. Generally, matrices are denoted by uppercase letter and vector as lowercase letter.
6. Scalar means object is a single value, not a vector or matrix.

## Addition and Scalar Multiplication 

Addition and subtraction are element-wise, So we simply add or subtract each corresponding element. For eg :
> A <br>
> a b <br>
> c d <br>
> <br>
> B <br>
> w x <br>
> y z <br>
> <br>
> A + B <br>
> a+w b+x <br>
> c+y d+z <br>
> <br>
> A - B <br>
> a-w b-x <br>
> c-y d-z <br>

To add or substract two matrices their dimensions must be the same. <br>
In scalar multiplication we simply multiply every element by scalar value.
> A x g <br>
> a x g b x g <br>
> c x g d x g <br>

In scalar division we simply divide every element by scalar value.
> A / g <br>
> a / g b / g <br>
> c / g d / g <br>

Eg : Let matrix A be :
> 1 0 <br>
> 2 5 <br>
> 3 1 <br>

So now 3 x A will be
> 1x3 0x3 = 3 0<br>
> 2x3 5x3 = 6 15<br>
> 3x3 1x3 = 9 3<br>
