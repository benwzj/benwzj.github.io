---
layout: post
title: Python Common Operation
date: 2024-12-15
categories: Python
tags: Python
toc: 
  - name: enumerate
  - name: hash(object)
  - name: Sort
  - name: Regular Expressions
---

## enumerate

`enumerate(iterable, start=0)`

Return an **enumerate object**. iterable must be a sequence, an iterator, or some other object which supports iteration. The `__next__()` method of the iterator returned by enumerate() returns a tuple containing a count (from start which defaults to 0) and the values obtained from iterating over iterable.

```py
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
list(enumerate(seasons))
#[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
list(enumerate(seasons, start=1))
#[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```
Equivalent to:
```py
def enumerate(iterable, start=0):
    n = start
    for elem in iterable:
        yield n, elem
        n += 1
```

### When use enumerate function

- When want something like this: `[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]`.
- Using Enumerate Object in Loops.

```py
my_list = ['apple', 'banana', 'cherry']
for index, value in enumerate(my_list):
    print(index, value)
# Output:
# 0 apple
# 1 banana
# 2 cherry
```

## hash(object)
Return the hash value of the object (if it has one). Hash values are **integers**. They are used to quickly compare dictionary keys during a dictionary lookup. 
> Numeric values that compare equal have the same hash value (even if they are of different types, as is the case for 1 and 1.0).


## Sort 

- In Python, Sort topic will focus on list.
- Python lists have a built-in list.sort() method that modifies the list in-place. 
- There is also a sorted() built-in function that builds a new sorted list from an iterable.

### Build-in function Sorted()

- sorted(iterable, /, *, key=None, reverse=False)
- sorted() sort iterable and returns a new sorted list.

#### key parameter
- Both list.sort() and sorted() have a key parameter 
- key parameter specify a function (or other callable) to be called on each list element prior to making comparisons.
- This function (or other callable) that takes a single argument and returns a key to use for sorting purposes. 
sorted("This is a test string from Andrew".split(), key=str.lower)
- Python provides convenience functions to make key parameter functions easier and faster. 
The operator module has itemgetter(), attrgetter(), and a methodcaller() function.
Using those functions, the above examples become simpler and faster:

#### reverse parameter 
with a boolean value.

#### Dictionary Sort
- You can sorted(dic) directly. It will sort by key.
- Common way to sort value in a dictionary

```py
import operator
d = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
sorted_l = sorted(d.items(), key=operator.itemgetter(1))
sorted_d = dict(sorted_l)
```

## Read and Write Files

### open() 

- open(filename, mode, encoding=None)
- returns a file object, and is most commonly used with two positional arguments and one keyword argument: 
- The first argument is a string containing the filename. 
- The second argument is another string containing a few characters describing the way in which the file will be used. 
- Appending a 'b' to the mode opens the file in binary mode. 
- Normally, files are opened in text mode, that means, you read and write strings from and to the file.
- It is good practice to use the `with` keyword when dealing with file objects.

```py
with open('workfile', encoding="utf-8") as f:
    read_data = f.read()
```

### f.read(size)
To read a file’s contents, call f.read(size), which reads some quantity of data and returns it as a string (in text mode) or bytes object (in binary mode). 
```py
f.read() 
'This is the entire file.\n'
```
### f.readline()
`f.readline()` reads a single line from the file;
```py
  f.readline()
  'This is the first line of the file.\n'
```
