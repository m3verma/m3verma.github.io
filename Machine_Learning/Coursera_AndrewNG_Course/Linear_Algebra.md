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

## Matrix Vector Multiplication

We map the column of the vector onto each row of the matrix, multiplying each element and summing the result. Suppose if we want to multiple Matrix (A) with vector (x) to generate vector (y). To get y<sub>i</sub>, multiply A's i<sup>th</sup> row with elements of vector x and add them up. So syntax will be :
> A<br>
> a b<br>
> c d<br>
> e f<br>
> x<br>
> x<br>
> y<br>
> A * x = y<br>
> a b<br>
> c d * x<br>
> e f * y<br>
> y<br>
> ax + by<br>
> cx + dy<br>
> ex + fy<br>

The result is a vector. Here we need to keep in mind that number of columns of matrix must be equal to number of rows of vector. If this condition is not satisfied, matrix vector multiplication cannot happen.
> [m x n] x [n x 1] = [m x 1]

An example :
> A<br>
> 1 3<br>
> 4 0<br>
> 2 1<br>
> x<br>
> 1<br>
> 5<br>
> A * x = y<br>
> 1x1 + 3x5 = 16<br>
> 4x1 + 0x5 = 4<br>
> 2x1 + 1x5 = 7<br>

Now we can use this matrix - vector multiplication in our hypothesis function. If you remember the house price prediction problem.
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x

Let's assume we got some value of θ<sub>0</sub> and θ<sub>1</sub>. So our hypothesis function will look like :
> h(x) = -40 + 0.25x

Now if we start making prediction for the below houses :
> 2104, 1416, 1534, 852

We will have to write a for loop to compute the hypothesis function for each value of our prediction. We can do it in a simpler way if our programming language supports matrix and vector multiplication. Let's assume matrix A is :
> A<br>
> 1 2104<br>
> 1 1416<br>
> 1 1534<br>
> 1 852<br>

We can make coeficient of hypothesis function as a vector x :
> x<br>
> -40<br>
> 0.25<br>

Now if we multiply :
> -40 + 0.25x2104<br>
> -40 + 0.25x1416<br>
> -40 + 0.25x1534<br>
> -40 + 0.25x852<br>

So this concept of multiplication we can get our prediction very easily.

## Matrix Matrix Multiplication

We multiply two matrices by breaking it into several vector multiplications and concatenating the result. Suppose if we want to multiple Matrix (A) with Matrix (B) to generate Matrix (C). To get y<sub>i</sub>, multiply A's i<sup>th</sup> row with i<sup>th</sup> columns of Matrix B and add them up. So syntax will be :
> A<br>
> a b<br>
> c d<br>
> e f<br>
> B<br>
> w x<br>
> y z<br>
> A * B = C<br>
> a b<br>
> c d * w x<br>
> e f * y z<br>
> y<br>
> aw + bx ax + bz<br>
> cw + dy cx + dz<br>
> ex + fy ex + fz<br>

The result is a Matrix. Here we need to keep in mind that number of columns of matrix must be equal to number of rows of second matrix. If this condition is not satisfied, matrix vector multiplication cannot happen.
> [m x n] x [n x o] = [m x o]

An example :
> A<br>
> 1 3<br>
> 2 5<br>
> B<br>
> 0 1<br>
> 3 2<br>
> A * B = C<br>
> 1x0 + 3x3 1x1 + 3x2 = 9 7<br>
> 2x0 + 5x3 2x1 + 5x2 = 15 12<br>

Now we can use this matrix - matrix multiplication in our multiple hypothesis function. If you remember the house price prediction problem.
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x

Let's assume we got some value of θ<sub>0</sub> and θ<sub>1</sub>. So our hypothesis function's will look like :
> h(x) = -40 + 0.25x
> h(x) = 200 + 0.1x
> h(x) = -150 + 0.4x

Now if we start making prediction for the below houses :
> 2104, 1416, 1534, 852

We will have to write two for loops to compute the hypothesis function for each value of our prediction. We can do it in a simpler way if our programming language supports matrix and matrix multiplication. Let's assume matrix A is :
> A<br>
> 1 2104<br>
> 1 1416<br>
> 1 1534<br>
> 1 852<br>

We can make coeficient of hypothesis function's as a matrix B :
> B<br>
> -40 200 -150<br>
> 0.25 0.1 0.4<br>

Now if we multiply :
> -40 + 0.25x2104 200 + 0.1x2104 -150 + 0.4x2104<br>
> -40 + 0.25x1416 200 + 0.1x1416 -150 + 0.4x1416<br>
> -40 + 0.25x1534 200 + 0.1x1534 -150 + 0.4x1534<br>
> -40 + 0.25x852  200 + 0.1x852  -150 + 0.4x852<br>

So this concept of multiplication we can get our prediction's for different hypothesis function very easily.
