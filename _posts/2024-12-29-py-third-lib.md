---
layout: post
title: Popular Third party lib For Python
date: 2024-12-29
categories: Python
tags: Python
toc: 
  - name: NumPy
  - name: SciPy
  - name: Pandas
  - name: Matplotlib
  - name: openpyxl
---

## NumPy

- NumPy is a Python library used for working with arrays.
- NumPy stands for Numerical Python.
- NumPy is the fundamental package for scientific computing in Python.
- It provides a multi-dimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays.

### Why Use NumPy?
- In Python we have lists that serve the purpose of arrays, but they are slow to process.
NumPy aims to provide an array object that is up to 50x faster than traditional Python lists.
- The array object in NumPy is called ndarray, it provides a lot of supporting functions that make working with ndarray very easy.
- NumPy arrays are stored at one continuous place in memory unlike lists, so processes can access and manipulate them very efficiently.
- NumPy is a Python library and is written partially in Python, but most of the parts that require fast computation are written in C or C++.

### Basic example:
```py
import numpy as np

arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
subarr = arr[0:2, 1:3]

print(subarr) like this:
[[2 3]
 [6 7]]

print(np.sum(arr))  # Output: 78
print(np.mean(arr, axis=1))  # Output: [ 2.5  6.5 10.5]
```

### Functions

np.random.normal

## SciPy

- SciPy is a scientific computation library that uses NumPy underneath.
- SciPy stands for Scientific Python.
- It provides more utility functions for optimization, stats and signal processing.
- Like NumPy, SciPy is open source so we can use it freely.
- SciPy was created by NumPy's creator Travis Olliphant.

### Why Use SciPy?
- If SciPy uses NumPy underneath, why can we not just use NumPy?
- SciPy has optimized and added functions that are frequently used in NumPy and Data Science.

## Pandas

Pandas is a Python library used for working with data sets.
It has functions for analyzing, cleaning, exploring, and manipulating data.
The name "Pandas" has a reference to both "Panel Data", and "Python Data Analysis" and was created by Wes McKinney in 2008.

### Why Use Pandas?
Pandas allows us to analyze big data and make conclusions based on statistical theories.
Pandas can clean messy data sets, and make them readable and relevant.
Relevant data is very important in data science.


## Matplotlib 

Matplotlib is a low level graph plotting library in python that serves as a visualization utility.
Most of the Matplotlib utilities lies under the pyplot submodule. 

What main job Matplotlib do as following: 
```py
import matplotlib.pyplot as plt
import numpy as np

xpoints = np.array([1, 4, 8])
ypoints = np.array([3, 5, 10])

plt.plot(xpoints, ypoints)
plt.show()
```
Then you get:
{% include figure.html path="assets/img/py-Matplotlib.jpg" class="img-fluid rounded z-depth-1" width="35%" %}

You can decorate this chart: like marker, line, label, grid, scatter, bars, histograms, pie.  

## openpyxl

(The first exercise)
https://openpyxl.readthedocs.io/en/stable/index.html#

openpyxl is a lib which deal with Excel 2010 xlsx/xlsm/xltx/xltm files.

### Create workbook
```bash
>>> from openpyxl import Workbook
>>> wb = Workbook()
>>> ws = wb.create_sheet("Mysheet") # insert at the end (default)
```

### Load data from a file
```bash
>>> from openpyxl import load_workbook
>>> wb2 = load_workbook('test.xlsx')
>>> print(wb2.sheetnames)
```
### Playing with cells content
```bash
>>> ws = wb.active
>>> ws['A4'] = 4
>>> d = ws.cell(row=4, column=2, value=10)
>>> for row in ws.iter_rows(min_row=1, max_col=3, max_row=2):
...    print(row) # one row is one tuple
...    for cell in row:
...        print(cell) # cell object: <Cell Sheet.A1>
```
### save data

`>>> wb.save('balances.xlsx')`

### Conclusion
- Lib documents supposed to support all information. Such as how to use it, Example, security.
- list, tuple, dictionary are basic object which are very good designed! and need to be familiar with. 


