---
layout: post
title: Python Type Overview
date: 2024-12-12
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

## List

- List is ordered and mutable, Allows duplicate members, similar to array in JavaScript.
- Lists are created using square brackets. Example: `list1 = ["abc", 34, True, 40, "male"]`
- If you want to determine something inside of a List, that is big O(n) time complexity. It look over most of the elements of the list. But for Set or Dict, it is constant time complexity.
- If you pop the last element of the list, it is constant time operation. But remove the first element, then it is O(n) operation.
- If you need to have an order collection of elements, you want to pop at the end or beginning or middle of it. You should use the following two data structure:
from collections import deque, queue

### list comprehension syntax

List comprehension is an elegant way to create lists based on existing lists.

Syntax:
```py
newlist = [expression for item in iterable if condition == True]
```

- A list comprehension consists of brackets containing an expression followed by a for clause, then zero or more for or if clauses.
- The result will be a new list resulting from evaluating the expression in the context of the for and if clauses which follow it. For example, this listcomp combines the elements of two lists if they are not equal:
`>>>[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]`
get:
`[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]`
- Key point: the first for loop is outer loop. But you can use bracket to wrap expression and for loop together, and make second for loop outside:

```py
matrix = [[1, 2], [3,4], [5,6], [7,8]]
# Flat 2D list to [1, 2, 3, 4, 5, 6, 7, 8]:
print([j for i in matrix for j in i])
# Transpose to [1, 3, 5, 7], [2, 4, 6, 8]]:
print([[row[i] for row in matrix] for i in range(2)])
```

#### The condition is like a filter, and it is optional:
```py
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
newlist = [x for x in fruits if "a" in x]
print(newlist) # ['apple', 'banana', 'mango']
```

#### The expression can also contain conditions, not like a filter, but as a way to manipulate the outcome:
```py
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
newlist = [x if x != "banana" else "orange" for x in fruits]
print(newlist) # fruits = ["apple", "orange", "cherry", "kiwi", "mango"]
```
#### Support nested list comprehension

### List methods

#### Access List items
- `s[i]`      ith item of s, origin 0
- `s[i:j]`    slice of s from i to j
- `s[i:j:k]`  slice of s from i to j with step k

#### Modifying List
- `append()` method
- `insert()` method
- support operator: +, *, +=, *=

#### Remove List Items
- remove() method removes the specified item
- pop() method removes the specified index.
- The del keyword also removes the specified index
- clear() method empties the list.

#### reverse()

#### sort() 
- It will sort the list alphanumerically, ascending, by default.
- Sort the list descending:
`lst.sort(reverse = True)`

#### Copy a list
Thre are 3 ways to copy a list: 
1. newlst = lst.copy()
2. newlst = list(lst)
3. newlst = [i for i in lst]

#### Join two list
1. list3 = list1 + list2
2. Another way to join two lists is by appending all the items from list2 into list1, one by one.
3. Use the extend() method to add list2 at the end of list1
`list1.extend(list2)`

#### Multi-Dimensional Arrays Operation
(When deal with Multi-Dimensional Arrays, it is good to use NumPy. )
What do X[:,1] means?
- X is a numpy array, and it is Multi-Dimensional Array.
- Say Here X have n rows and n columns
- so by doing x=x[:,1] we get all the rows in x present at index 1.

For example:
```py
x = array([ [0.69859393, 0.1042432 ],
         [0.55138493, 0.18639614],
         [0.27338772, 0.80351282]])

x[:,1] = array([0.1042432 , 0.18639614, 0.80351282])
```

### Understanding Lists in Python

#### What is Array in Python
- An array is a set of elements which 
  - have the same size, 
  - located in memory one after another, without gaps.

- Since the “get value by address” memory operation takes constant time, selecting an array item by index also takes O(1).

#### Array vs. List
{% include figure.html path="assets/img/python-list.jpg" class="img-fluid rounded z-depth-1" width="100%" %}

#### List is kind of array
The list is based on the array. 

##### List = Array of Pointers
- The list instantly retrieves an item by index (O(1)), because it has an array inside. 
- And the array is so fast because all the elements are the same size.

For example:
```bash
>>>sys.getsizeof(['1'])
64
>>>sys.getsizeof(['1','2'])
72
>>>sys.getsizeof(['a long long long string'])
64
```

##### List = Dynamic Array
- The list juggles arrays all the time so that we don’t have to do it . For example, if array is full, it will allocate new memory for all elements. 
- You can even check all detail in Python c code.

##### these operations are O(1):
- select an item by index lst[idx]
- count items len(lst)
- add an item to the end of the list .append(item)
- remove an item from the end of the list .pop()

##### operations are “slow”:
- Insert or delete an item by index. .insert(idx, item) and .pop(idx) take linear time `O(n)` because they shift all the elements after the target one.
- Search or delete an item by value. item in lst, .index(item) and .remove(item) take linear time `O(n)` because they iterate over all the elements.
- Select a slice of k elements. l`st[from:to]` takes `O(k)`.

### List questions
- why Memory of list is like that: it depond on how many members instead of how big of the members? 
- why some operations are quick, but some are slow?
- Do Python allocate new memory for the following variable small_list?
big_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
small_list = big_list[:5]
Ask yourself if you modify small_list, do big_list modify as well? if not, then new memory.

