---
layout: post
title: "Iterable Iterator concept in Python"
date: 2023-05-13
categories: Python
---

**In short:**
Iterbale support **iter**, and maintain the data.
Iterator support **next**, and reach the data.

# Iterable

## Official defination

Any object that supports _iter()_ and return iterator is said to be "iterable."
example:

- list, str, and tuple are classical example of iterables
- Some non-sequence types like dict, file objects are iterables.
- Objects of any classes you define with an **iter**() method or with a **getitem**() method that implements sequence semantics are iterables.

Iterables can be used in a for loop and in many other places where a sequence is needed.  
built-in function iter(), it returns an iterator for the object.

# Iterator

## Official definition

The iterator objects themselves are required to support the following two methods, which together form the iterator protocol:
iterator.**iter**() return itself.
iterator.**next**() return one data and maintain state.

**But CPython doesn't consistently apply**

# Iterable vs. Iterator

```python
lst = [1,2,3]
i_lst = iter(lst)
```

- lst is iterable, but not iterator.
- i_lst is iterator and also iterable. i_lst is list_iterator object

An iterable can returns a **fresh** ITERATOR.
An iterator can return itself.
And iterator also is an object with a **next** method that returns the next value in the iteration and updates the state to point at the next value

# Pure iterables

Maybe we can produce a pure iterable concept.
Pure iterable typically hold the data itself, and return fresh iterator.
In contrast, iterator is not pure iterable that fetch data and return itself.

# Conclusion

Many people say iterators are iterables as well, and iterables don't have to be iterators.
But they also say iterators and iterables are different, like iterators are more effecient in memory consumsion.
That is confusing concept.
I reckon iterator don't have to support **iter**(). That means iterator don't have to be iterable. Just like JavaScript.
