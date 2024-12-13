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
{% include figure.html path="assets/img/python-list.jpg" class="img-fluid rounded z-depth-1" width="60%" %}

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


## Tuple 

### What is Tuple

- A tuple is a sequence which is ordered and Immutable, Allows duplicate.
- Tuples are unchangeable, meaning that we cannot change, add or remove items after the tuple has been created. However, if a member of the tuple is list, list content can be changed. 
- But there is a workaround. You can convert the tuple into a list, change the list, and convert the list back into a tuple.
- Tuples are written with round brackets:
tuple1 = ("abc", 34, True, 40, "male")

### Tuple Features

- Tuple is immutable, but it will instantiated without checking memory. It don’t like many immutable, e.g. int, float, check memory for same value instance. 
- Tuple have quirk syntax when it contain 0 or 1 items. 
t = ()  
t = (1,)

### Access Tuple. 
- using []. 
- using keyword ‘in’.

### Update Tuple
Convert the tuple into a list, update, and convert it back into a tuple.

### Unpacking a Tuple
When create a tuple, normally assign values to it. This is called "packing" a tuple.
Tuple allowed to extract the values back into variables. This is called "unpacking". (Call destructuring assignment in JavaScript)
```py
fruits = ("apple", "banana", "cherry")
(green, yellow, red) = fruits
```
Add an * to the variable name and the values will be assigned to the variable as a list:
```py
fruits = ("apple", "banana", "cherry", "strawberry", "raspberry")
(green, yellow, *red) = fruits
```

### You join Tuples by using ‘+’, multiply Tuples by using ‘*’


## Set

### What is Set
A set object is an unordered collection of distinct hashable objects.
Like other collections, sets support x in set, len(set), and for x in set. 
Python have set and frozen set.

- Set support comprehension syntax as well, similar to list comprehension.

### Common uses include 
- membership testing, 
- removing duplicates from a sequence, 
- Provide computing mathematical operations such as intersection, union, difference, and symmetric difference. 

### set
These represent a mutable — the contents can be changed using methods like add() and remove(). 
Since it is mutable, it has no hash value and cannot be used as either a dictionary key or as an element of another set. 

- A set is a collection which is unordered, Not allow duplicates.
- We cannot change the items after the set has been created. but you can remove items and add new items.
- Sets are written with curly brackets:
set1 = {"abc", 34, True, 40, "male"}

### frozenset
These represent an immutable set. 
As a frozenset is immutable and hashable, it can be used again as an element of another set, or as a dictionary key.

### Instances of set and frozenset provide the following operations:
(You can tell what set and frozenset can do according this operations)

- `len(s)`
Return the number of elements in set s (cardinality of s).
- `x in s`
Test x for membership in s.
- `x not in s`
Test x for non-membership in s.
- `isdisjoint(other)`
Return True if the set has no elements in common with other. Sets are disjoint if and only if their intersection is the empty set.
- `issubset(other)`: `set <= other`
Test whether every element in the set is in other.
- `set < other`
Test whether the set is a proper subset of other, that is, set <= other and set != other.
- `issuperset(other)` : 
`set >= other`
Test whether every element in other is in the set.
-` set > other`
Test whether the set is a proper superset of other, that is, set >= other and set != other.
- `union(*others)`
`set | other | ...`
Return a new set with elements from the set and all others.
- `intersection(*others)`
`set & other & ...`
Return a new set with elements common to the set and all others.
- `difference(*others)`
`set - other - ...`
Return a new set with elements in the set that are not in the others.
- `symmetric_difference(other)`
`set ^ other`
Return a new set with elements in either the set or other but not both.
- `copy()`
Return a shallow copy of the set.

### operations available for set, but not frozenset

update(*others)
set |= other | ...
Update the set, adding elements from all others.

intersection_update(*others)
set &= other & ...
Update the set, keeping only elements found in it and all others.

difference_update(*others)
set -= other | ...
Update the set, removing elements found in others.

symmetric_difference_update(other)
set ^= other
Update the set, keeping only elements found in either set, but not in both.

add(elem)
Add element elem to the set.

remove(elem)
Remove element elem from the set. Raises KeyError if elem is not contained in the set.

discard(elem)
Remove element elem from the set if it is present.

pop()
Remove and return an arbitrary element from the set. Raises KeyError if the set is empty.

clear()
Remove all elements from the set.

### Questions

#### Can Set contains mutable type, like list? 
NO, elements in set, key in dictionary, have to be hashable. That means it can’t be list or other mutable type. Tuple can be elements of set if it haven’t mutable member.

## Dictionary

### What is Dictionary

Dictionaries are used to store data values in `key:value` pairs.
A dictionary is a collection which is ordered*, mutable and do not allow duplicates.

### Features

- keys, which can be any immutable type; strings and numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple contains any mutable object either directly or indirectly, it cannot be used as a key. 
- As of Python version 3.7, dictionaries are ordered. In Python 3.6 and earlier, dictionaries are unordered.
- Use curl bracket.
- The dict() constructor builds dictionaries directly from sequences of key-value pairs:
`dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])` get
`{'sape': 4139, 'guido': 4127, 'jack': 4098}`
- dict comprehensions can be used to create dictionaries from arbitrary key and value expressions:
`{x: x**2 for x in (2, 4, 6)}` get
`{2: 4, 4: 16, 6: 36}`

### Accessing Items

- Dictionary is in order. But have no idea to access it by order.
Access directly:
```py
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
}
print(thisdict["model"])
```
- using square bracket [] is equivalent to `__getitem__()`
- if visit key that doesn’t exist, It will raise KeyError.
But you can override the `__missing__(self, key)` method defines the behavior of a dictionary subclass if you access a non-existent key. 
More specifically, Python’s `__getitem__()` dictionary method internally calls the `__missing__()` method if the key doesn’t exist. The return value of `__missing__()` is the value to be returned when trying to access a non-existent key.
- keys(), values(), items() 
- return view objects for corresponded things. 
- Dictionary view object provide a dynamic view on the dictionary’s entries, which means that when the dictionary changes, the view reflects these changes.
- Keys views are set-like!
- If all values are hashable, then the items view is also set-like.
- Values views are not treated as set-like.
- For set-like views, all of the operations defined for the abstract base class collections.abc.Set are available (for example, ==, <, or ^).

- setdefault(key[, default])
Returns the value of the specified key. If the key does not exist: insert the key, with the specified value. 
  - keyname is required
  - value is Optional. If the key exist, value has no effect.
If the key does not exist, this value becomes the key's value. Default value None. 
`print(thisdict.setdefault("model","Focus"))`

- get(key[, default])
Return the value for key if key is in the dictionary, else default. If default is not given, it defaults to None.
  - get() is different function from `__getitem__()`. They have no connection.

### Remove items

- pop()
The pop() method removes the item with the specified key name.
- popitem()
The popitem() method removes the last inserted item (in versions before 3.7, a random item is removed instead):
- del keyword
Can delete items or the dictionary
- clear()
Clear the dictionary

### Add items, Change items

- add item directly, change directly
(list can’t add item directly, but dict can)
```py
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
}
thisdict["color"] = "red"
thisdict["model"] = "Focus"
```

- using square bracket [] is equivalent to `__setitem__()`

### update()
The update() method inserts the specified items to the dictionary.
- update() can overwrite the exist key and it’s value. 
- can concatenate dictionarys. 
- can concatenate key value pair iterable object.
dictionary.update(iterable)
 • iterable can be A dictionary or an iterable object with key value pairs, that will be inserted to the dictionary. 

### dict NOT support operator +
But Counter can
- setdefault()
see above

### Create Dictionary

- dict.fromkeys(keys, value)
The fromkeys() method returns a dictionary with the specified keys and the specified value.

- syntac
`dict.fromkeys(keys, value)`
keys: Required. An iterable specifying the keys of the new dictionary
value: Optional. The value for all keys. Default value is None

- an example of how to use dict as an ordered set to filter out duplicate items while preserving order, thereby emulating an ordered set:
`keywords = ['foo', 'bar', 'bar', 'foo', 'baz', 'foo']`
`list(dict.fromkeys(keywords))`
`['foo', 'bar', 'baz']`

- zip()
zip() returns an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument iterables.
```py
keys = ['red', 'green', 'blue']
values = ['#FF0000','#008000', '#0000FF']
color_dictionary = dict(zip(keys, values))
```

### Copy Dictionary
- copy()
Dictionary method copy() can create a new dict according to the current dict.

- dict()
Another way to make a copy is to use the built-in function dict().

- Loop over dict
```py
for x in thisdict:
  print(x)
```
x is the key! no value. 

### Question

- How to get the first item of a dict?


## Time Complexity of Collections

### list

- Internally, a list is represented as an array; 
- The largest costs come from growing beyond the current allocation size (because everything must move), or from inserting or deleting somewhere near the beginning (because everything after that must move). 
- If you need to add/remove at both ends, consider using a collections.deque instead.

{% include figure.html path="assets/img/python-list-timecomplexity.jpg" class="img-fluid rounded z-depth-1" width="30%" %}

### dict 

- There is a fast-path for dicts that (in practice) only deal with str keys; 
- Python have a typical space-time tradeoff in dictionaries and lists. It means we can decrease the time necessary for our algorithm but we need to use more space in memory for dictionaries.
- time complexity x in s operation for dict is O(1); But for list if O(n).

{% include figure.html path="assets/img/python-dict-timecomplexity.jpg" class="img-fluid rounded z-depth-1" width="40%" %}

### set

See dict -- the implementation is intentionally very similar.

{% include figure.html path="assets/img/python-set-timecomplexity.jpg" class="img-fluid rounded z-depth-1" width="70%" %}

### collections.deque

A deque (double-ended queue) is represented internally as a doubly linked list. (Well, a list of arrays rather than objects, for greater efficiency.) Both ends are accessible, but even looking at the middle is slow, and adding to or removing from the middle is slower still.

{% include figure.html path="assets/img/python-deque-timecomplexity.jpg" class="img-fluid rounded z-depth-1" width="40%" %}
