---
layout: post
title: Some Build-in lib in Python
date: 2024-12-29
categories: Python
tags: Python
toc: 
  - name: heapq
  - name: json
  - name: io module 
---


## heapq

(Still don’t get the main point of this module and heap)

This module provides an implementation of the heap queue algorithm, also known as the priority queue algorithm.

### What is heap

- Heaps are binary trees for which every parent node has a value less than or equal to any of its children. 
- Heaps are arrays for which `a[k] <= a[2*k+1] and a[k] <= a[2*k+2]` for all k, counting elements from 0. 
- For the sake of comparison, non-existing elements are considered to be infinite. 
- The interesting property of a heap is that its smallest element is always the root, `heap[0]`.
- In a word, heaps are useful memory structures to know.

### functions
#### heapq.nlargest(n, iterable, key=None)
Return a list with the n largest elements from the dataset defined by iterable. 

## json

json exposes an API familiar to users of the standard library marshal and pickle modules.

### Functions
#### json.loads() and json.load()

conversion table: 
{% include figure.html path="assets/img/py-json-load.jpg" class="img-fluid rounded z-depth-1" width="35%" %}

#### json.loads(s, *)
Deserialize s (a str, bytes or bytearray instance containing a JSON document) to a Python object using this conversion table.

For example, If you have a JSON string, you can parse it by using the json.loads() method. it return a Python dictionary.
```py
x = '{"name":"John", "age":30, "city":"New York"}'
dct = json.loads(x)
```
#### json.load(fp, *)

Deserialize fp (a .read()-supporting text file or binary file containing a JSON document) to a Python object using this conversion table.

#### json.dumps(obj, *)
Serialize obj to a JSON formatted str using this conversion table. 
#### json.dump(obj, fp, *)
Serialize obj as a JSON formatted stream to fp (a .write()-supporting file-like object) using this conversion table.

### Classes

You can create subclass of these defined classes: 

#### class json.JSONDecoder

Simple JSON decoder.

#### method: decode(s)
Return the Python representation of s (a str instance containing a JSON document).

#### class json.JSONEncoder

Extensible JSON encoder for Python data structures.

#### method: default(o)
Implement this method in a subclass such that it returns a serializable object for o, or calls the base implementation (to raise a TypeError).
#### encode(o)
Return a JSON string representation of a Python data structure, o.
#### iterencode(o)
Encode the given object, o, and yield each string representation as available.


## io module 

- The io module provides Python’s main facilities for dealing with various types of I/O. 
- There are three main types of I/O: text I/O, binary I/O and raw I/O.
- A concrete object belonging to any of these categories is called a file object. Other common terms are stream and file-like object.

### Text I/O

Text I/O expects and produces str objects. 
The easiest way to create a text stream is with open():
`f = open("myfile.txt", "r", encoding="utf-8")`

In-memory text streams are also available as StringIO objects:
`f = io.StringIO("some initial text data")`

### Binary I/O

Binary I/O (also called buffered I/O) expects bytes-like objects and produces bytes objects.

The easiest way to create a binary stream is with open() with 'b' in the mode string:
`f = open("myfile.jpg", "rb")`

In-memory binary streams are also available as BytesIO objects:
`f = io.BytesIO(b"some initial binary data: \x00\x01")`