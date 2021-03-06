---
layout: default
---

# Preliminaries

## What kinds of Data?

The primary focus is on structured data, a deliberately vague term that encompasses many different common forms of data, such as :

1. Tabular or spreadsheet-like data in which each column may be a different type (string, numeric, date, or otherwise). This includes most kinds of data commonly stored in relational databases or tab- or comma-delimited text files.
2. Multidimensional arrays (matrices).
3. Multiple tables of data interrelated by key columns (what would be primary or foreign keys for a SQL user).
4. Evenly or unevenly spaced time series.

## Why Python for Data Analysis?

Python has developed a large and active scientific computing and data analysis community. In the last 10 years, Python has gone from a bleeding-edge or “at your own risk” scientific computing language to one of the most important languages for data science, machine learning, and general software development in academia and industry. In recent years, Python’s improved support for libraries (such as pandas and scikit-learn) has made it a popular choice for data analysis tasks. Combined with Python’s overall strength for general-purpose software engineering, it is an excellent option as a primary language for building data applications.

1. Python as Glue - Part of Python’s success in scientific computing is the ease of integrating C, C++, and FORTRAN code.
2. Solving the "Two-Language" Problem - What people are increasingly finding is that Python is a suitable language not only for doing research and prototyping but also for building the production systems.
3. Why Not Python? - As Python is an interpreted programming language, in general most Python code will run substantially slower than code written in a compiled language like Java or C++. Python can be a challenging language for building highly concurrent, multithreaded applications, particularly applications with many CPU-bound threads.

## Essential Python Libraries

### NumPy

NumPy, short for Numerical Python, has long been a cornerstone of numerical computing in Python. It provides the data structures, algorithms, and library glue needed for most scientific applications involving numerical data in Python. NumPy contains, among other things:

1. A fast and efficient multidimensional array object ndarray
2. Functions for performing element-wise computations with arrays or mathematical operations between arrays
3. Tools for reading and writing array-based datasets to disk
4. Linear algebra operations, Fourier transform, and random number generation
5. A mature C API to enable Python extensions and native C or C++ code to access NumPy’s data structures and computational facilities

For numerical data, NumPy arrays are more efficient for storing and manipulating data than the other built-in Python data structures. Also, libraries written in a lower-level language, such as C or Fortran, can operate on the data stored in a NumPy array without copying data into some other memory representation.

### pandas

pandas provides high-level data structures and functions designed to make working with structured or tabular data fast, easy, and expressive. Since its emergence in 2010, it has helped enable Python to be a powerful and productive data analysis environment. pandas blends the high-performance, array-computing ideas of NumPy with the flexible data manipulation capabilities of spreadsheets and relational databases.It provides sophisticated indexing functionality to make it easy to reshape, slice and dice, perform aggregations, and select subsets of data. The pandas name itself is derived from panel data, an econometrics term for multidimensional structured datasets, and a play on the phrase Python data analysis itself.

### matplotlib

matplotlib is the most popular Python library for producing plots and other two-dimensional data visualizations. It was originally created by John D. Hunter and is now maintained by a large team of developers. It is designed for creating plots suitable for publication.

### IPython and Jupyter

It does not provide any computational or data analytical tools by itself, IPython is designed from the ground up to maximize your productivity in both interactive computing and software development. It encourages an execute-explore workflow instead of the typical edit-compile-run workflow of many other programming languages. It also provides easy access to your operating system’s shell and filesystem. The Jupyter notebook system also allows you to author content in Markdown and HTML, providing you a means to create rich documents with code and text. Other programming languages have also implemented kernels for Jupyter to enable you to use languages other than Python in Jupyter.

### SciPy

SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing. Here is a sampling of the packages included. Together NumPy and SciPy form a reasonably complete and mature computational foundation for many traditional scientific computing applications.

### scikit-learn

scikit-learn has become the premier general-purpose machine learning toolkit for Python programmers. In just seven years, it has had over 1,500 contributors from around the world. It includes submodules for such models as :

1. Classification: SVM, nearest neighbors, random forest, logistic regression, etc.
2. Regression: Lasso, ridge regression, etc.
3. Clustering: k-means, spectral clustering, etc.
4. Dimensionality reduction: PCA, feature selection, matrix factorization, etc.
5. Model selection: Grid search, cross-validation, metrics
6. Preprocessing: Feature extraction, normalization

## Installation and Setup

To install on Windows, download the [Anaconda installer](https://www.anaconda.com/products/individual). Follow the instructions on the link to install anaconda in your system. To install additional Python packages :

```python
conda install package_name
pip install package_name
conda update package_name
pip install --upgrade package_name
```

### Integrated Development Environments (IDEs) and Text Editors

When building software, however, some users may prefer to use a more richly featured IDE rather than a comparatively primitive text editor like Emacs or Vim. Here are some that you can explore :
1. PyDev (free), an IDE built on the Eclipse platform
2. PyCharm from JetBrains (subscription-based for commercial users, free for open source developers)
3. Python Tools for Visual Studio (for Windows users)
4. Spyder (free), an IDE currently shipped with Anaconda
5. Komodo IDE (commercial)

### Import Conventions

The Python community has adopted a number of naming conventions for commonly used modules :

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import statsmodels as sm
```

This means that when you see np.arange, this is a reference to the arange function in NumPy. This is done because it’s considered bad practice in Python software development to import everything (from numpy import *) from a large package like NumPy.
