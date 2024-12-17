---
layout: post
title: Python Build-in Functions
date: 2024-12-15
categories: Python
tags: Python
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
