---
layout: post
title: "How <code>for loop</code> iterate an iterable in Python"
date: 2023-05-12
categories: Python
tags: Iteration Python
---


> ##### Important: 
> `for loop` iterating an iterable is a lazy processing.
{: .block-warning }

## Understand this lazy processing

I am going to make two example to display how `for...in` loop work underneath the cover.  
First one is loop over a enumerate object, and second one is loop over a range object. Both of them will modify the list inside the loop.

### First example:

```python
lst = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

for i, value in enumerate(lst):
   lst.pop(i)
print(lst)
```

> There are No error raising for these codes, but they are not work as expected.
{: .block-danger }

The code print below:

```python
[1, 3, 5, 7, 9]
```

enumerate object is generator object which will yield value from `lst`. It is lazy process.
When `lst` change, `value` from enumerate object change as well, but `i` is keep going.

**for...in is equivalent to:**
```python
_iter = iter(enumerate_obj)
while 1:
 try:
   x = _iter.__next__()
 except StopIteration:
   break
 # statements
```

**enumerate() is equivalent to:**
```python
def enumerate(lst):
    n = 0
    for i in lst:
        yield n, i
        n += 1
```

### Second example:

```python
lst = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

for i in range(len(lst)):
   lst.pop(i)
```

**This code will raise IndexError**

_range()_ is iterable, but _len(lst)_, the parameter of _range()_, have been replace with constant value 10. Because _len(lst)_ is not a lazy process. It don’t change any more. So _i_ will up to 9.

**The codes are equivalent to:**

```python
for i in range(9):
   lst.pop(i)
```
