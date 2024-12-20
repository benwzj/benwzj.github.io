---
layout: post
title: Python Sequence Type
date: 2024-12-15
categories: Python
tags: Python String
toc: 
  - name: What is Python Sequence
  - name: Common Sequence Operations
  - name: Understand Binary Sequence
    subsections: 
      - name: Bytes objects and ASCII text
      - name: Bytes and Strings
      - name: Bytearray Objects
  - name: Array
  - name: References
---

## What is Python Sequence

In Python, when accessing an attribute of an object, Python is **duck-typed**. The object will only be checked to see if it has an attribute at runtime, and only when immediately requested.

However, Python also has **nominal typing** features. Nominal typing is where one type is declared to be a subclass of another. 

When using duck-typing, the definition can be: 
A sequence(little s) is any ordered collection of objects which supports efficient element access using integer indices via the `__getitem__()` special method and defines a `__len__()` method that returns the length of the sequence. 

For Nominal typing, using `collections.abc.Sequence` as base class. 

For example: 
A numpy array is a sequence, but it is not a Sequence as it is not registered as a subclass of Sequence. 
```py
isinstance(np.arange(6),collections.abc.Sequence)
# False 
```

### There are three basic sequence types: 
- lists, typically used to store collections of homogeneous items
- tuples, typically used to store collections of heterogeneous data 
- range objects, represents an immutable sequence of numbers and is commonly used for looping a specific number of times in `for` loops.

### Here are more:
- Strings: Textual data in Python is handled with `str` objects, or strings. Strings are immutable sequences of Unicode code points. 
- Bytes objects: are immutable sequences of single bytes.
- Bytearray objects: are a mutable counterpart to bytes objects.
- Immutable sequence types: support for the `hash()` built-in.

## Common Sequence Operations

The `collections.abc.Sequence` ABC is provided to make it easier to correctly implement these operations on custom sequence types.

```py
x in s
x not in s
s + t
s * n
s[i]
s[i:j]
s[i:j:k]
len(s)
min(s)
max(s)
s.index(x[, i[, j]])
s.count(x)
```

## Understand Binary Sequence

The core built-in types for manipulating binary data are `bytes` and `bytearray`. 
They are supported by `memoryview` which uses the buffer protocol to access the memory of other binary objects without needing to make a copy.

- Bytes objects: are immutable sequences of single bytes.
- Bytearray objects: are a mutable counterpart to bytes objects.

### Why we need Bytes object
The only thing that a computer can store is bytes.
To store anything in a computer, you must first encode it, i.e. convert it to bytes. For example:
- If you want to store music, you must first encode it using MP3, WAV, etc.
- If you want to store a picture, you must first encode it using PNG, JPEG, etc.
- If you want to store text, you must first encode it using ASCII, UTF-8, etc.
MP3, WAV, PNG, JPEG, ASCII and UTF-8 are examples of encodings. An encoding is a format to represent audio, images, text, etc. in bytes.

In Python, everything must be converted to a Binary Sequence before it can be stored in a computer.
Binary Sequence, sometime can be represented by a byte string, based on text.

### Bytes objects and ASCII text
Bytes literals and representations can be based on ASCII text, While it is based on text, Only ASCII characters are permitted in bytes literals. Any binary values over 127 must be entered into bytes literals using the appropriate escape sequence.

And while Bytes literals based on text, bytes objects actually behave like immutable sequences of integers, with each value in the sequence restricted such that `0 <= x < 256` (attempts to violate this restriction will trigger `ValueError`).

The syntax for bytes literals is largely the same as that for string literals, except that a `b` prefix is added:
Single quotes: `b'still allows embedded "double" quotes'`

### Ways of Creating Bytes objects other than literal forms
In addition to the literal forms, bytes objects can be created in a number of other ways:
- A zero-filled bytes object of a specified length: `bytes(10)`
- From an iterable of integers: `bytes(range(20))`
- Copying existing binary data via the buffer protocol: `bytes(obj)`

#### classmethod fromhex(string)
This bytes class method returns a bytes object, decoding the given string object. The string must contain two hexadecimal digits per byte, with ASCII whitespace being ignored.
```bash
>>> bytes.fromhex('2Ef0 F1f2  ')
b'.\xf0\xf1\xf2'
```

#### hex()
Return a string object containing two hexadecimal digits for each byte in the instance.
```bash
>>> b'\xf0\xf1\xf2'.hex()
'f0f1f2'
```

### Bytes and Strings

In Python 3, there is a much cleaner separation between text and binary data. Text is always Unicode and is represented by the `str` type, and binary data is represented by the `bytes` type. What makes the separation particularly clean is that `str` and `bytes` can't be mixed in Python 3 in any implicit way. 

- The `bytes` type is a sequence of bytes that have been encoded and are ready to be stored in memory/disk. There are many types of encodings (utf-8, utf-16, also PNG, JPEG, MP3, WAV etc.), which all handle the bytes differently. The bytes object can be decoded into a `str` type or others.

- The `str` type is a sequence of **unicode** characters. The str needs to be encoded to be stored, but is mutable and an abstraction of the bytes logic. (Unicode Sandwich theory!)

- There is a strong relationship between `str` and `bytes`. `bytes` can be decoded into a `str`, and `str`s can be encoded into `bytes`.

```bash
>>> '€20'.encode('utf-8')
b'\xe2\x82\xac20'
>>> b'\xe2\x82\xac20'.decode('utf-8')
'€20'
```

### Bytearray Objects

bytearray object share all behavior with bytes except for it’s **mutability**.

There is no dedicated literal syntax for bytearray objects, instead they are always created by calling the constructor:
- Creating an empty instance: `bytearray()`
- Creating a zero-filled instance with a given length: `bytearray(10)`
- From an iterable of integers: `bytearray(range(20))`
- Copying existing binary data via the buffer protocol: `bytearray(b'Hi!')`

Bytearray Objects Support classmethod `fromhex(string)` and `hex()` as well. akin to bytes. 

## Array

Arrays are sequence types and behave very much like lists, except that **the type of objects stored in them is constrained**. 
Array represent an array of basic values, like characters, integers, floating-point numbers. 

The type of the object which Array contain is specified at object creation time by using a `typecode`.
`typecode` is a single character. 
For example, 'l' means `signed long`.  You  can do this: `array('l', [1, 2, 3, 4, 5])`.

> The NumPy package defines another array type.

## NumPy array

- In Python we have lists that serve the purpose of arrays, but they are slow to process.
NumPy aims to provide an array object that is up to 50x faster than traditional Python lists.
- The array object in NumPy is called ndarray, it provides a lot of supporting functions that make working with ndarray very easy.
- NumPy arrays are stored at one continuous place in memory unlike lists, so processes can access and manipulate them very efficiently.
- NumPy is a Python library and is written partially in Python, but most of the parts that require fast computation are written in C or C++.

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

## References

- [Python official doc on Array](https://docs.python.org/3/library/array.html).


