---
layout: post
title: Python Sequence Type
date: 2024-12-15
categories: Python
tags: Python String
---

There are three basic sequence types: 
- lists, typically used to store collections of homogeneous items
- tuples, typically used to store collections of heterogeneous data 
- range objects, represents an immutable sequence of numbers and is commonly used for looping a specific number of times in `for` loops.

Here are some example:
- Immutable sequence types: support for the `hash()` built-in.
- Strings: Textual data in Python is handled with `str` objects, or strings. Strings are immutable sequences of Unicode code points. 
- Bytes objects: are immutable sequences of single bytes.
- Bytearray objects: are a mutable counterpart to bytes objects.

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

The core built-in types for manipulating binary data are `bytes` and `bytearray`. They are supported by `memoryview` which uses the buffer protocol to access the memory of other binary objects without needing to make a copy.

### Bytes Objects

- Bytes Objects is 

The syntax for bytes literals is largely the same as that for string literals, except that a `b` prefix is added:
Single quotes: `b'still allows embedded "double" quotes'`

> Only **ASCII characters** are permitted in bytes literals (regardless of the declared source code encoding). Any binary values over 127 must be entered into bytes literals using the appropriate escape sequence.

bytes object share all behavior with bytearray except for it’s immutability.

#### In addition to the literal forms, bytes objects can be created in a number of other ways:
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

### Bytearray Objects

There is no dedicated literal syntax for bytearray objects, instead they are always created by calling the constructor:
- Creating an empty instance: `bytearray()`
- Creating a zero-filled instance with a given length: `bytearray(10)`
- From an iterable of integers: `bytearray(range(20))`
- Copying existing binary data via the buffer protocol: `bytearray(b'Hi!')`

Bytearray Objects Support classmethod `fromhex(string)` and `hex()` as well. akin to bytes. 


