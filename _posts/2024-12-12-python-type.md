---
layout: post
title: Python Type Overview
date: 2024-12-11
categories: Python
tags: Python
---

## Concepts of Containers, Collections, Sequences, Mappings

- Containers > Collections > Sequences
- Container concept is under the context of Object: Some objects contain references to other objects; these are called containers. Examples of containers are tuples, lists and dictionaries. The references are part of a container’s value. 
- Collection: data structures that store and organize data. They are container data types that allow you to add, remove, and iterate over items. Python has 4 built-in data structures that can be used to hold a collection of objects, they are 
  - list
  - tuple
  - set 
  - dictionary
- Containers or Collections are objects that are used to store different objects and provide a way to access the contained objects and iterate over them.
  - Python Standard Library provide **collections module** to implement specialized container datatypes providing alternatives to Python’s general purpose built-in containers, dict, list, set, and tuple.
  - You can use the `counter` function to count the number of occurrences of each element in a container.
- A sequence is an **ordered** collection. They maintain the order of the things in them. (set, dict are NOT sequence). Sequences are a special type of iterable that can be indexed using square brackets `[...]` to get items by their position. You can also ask sequences for their length to see how many things are inside them. 
  - List
  - Tuple
  - string
  - byte 
  - range
- A mapping object maps hashable values to arbitrary objects. Mappings are mutable objects. There is currently only one standard mapping type, the dictionary.

## Collection



