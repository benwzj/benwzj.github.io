---
layout: post
title: "Iteration in Python"
date: 2023-05-13
categories: Python
tags: Iterable Iterator Iteration Generator
---

**In short:**
Iterbale support `iter()` and maintain the data.
Iterator support `next()` and reach the data.

## Iterable

### Official defination

Any object that supports `iter()` and return iterator is said to be "iterable."
example:

- list, str, and tuple are classical example of iterables
- Some non-sequence types like dict, file objects are iterables.
- Objects of any classes you define with an `iter()` method or with a `getitem()` method that implements sequence semantics are iterables.

Iterables can be used in a for loop and in many other places where a sequence is needed.  
built-in function `iter()`, it returns an iterator for the object.

## Iterator

### Official definition

The iterator objects themselves are required to support the following two methods, which together form the iterator protocol:
`iterator.iter()` return itself.
`iterator.next()` return one data and maintain state.

**But CPython doesn't consistently apply**

## Iterable vs. Iterator

```python
lst = [1,2,3]
i_lst = iter(lst)
```

- lst is iterable, but not iterator.
- i_lst is iterator and also iterable. i_lst is list_iterator object

An iterable can returns a **fresh** ITERATOR.
An iterator can return itself.
And iterator also is an object with a **next** method that returns the next value in the iteration and updates the state to point at the next value

## Pure iterables

Maybe we can produce a pure iterable concept.
Pure iterable typically hold the data itself, and return fresh iterator.
In contrast, iterator is not pure iterable that fetch data and return itself.

## Conclusion

Many people say iterators are iterables as well, and iterables don't have to be iterators.
But they also say iterators and iterables are different, like iterators are more effecient in memory consumsion.
That is confusing concept.
I reckon iterator don't have to support `iter()` That means iterator don't have to be iterable. Just like JavaScript.

## Generator

### What is Generator

- Generators are a special class of functions that simplify the task of writing iterators. 
- Generator functions is a convenient shortcut to building iterators.
- Regular functions compute a value and return it, but generators return an iterator that returns a stream of values.
- When you call a generator function, it doesn’t return a single value; instead it returns a generator object that supports the iterator protocol.
- If a container object’s `__iter__()` method is implemented as a generator, it will automatically return a generator object supplying the `__iter__()` and `__next__()` methods. 
- There are two ways to create Generator: generator expression and generator function
- If you create a list to sum the first n. When n is really big, then it consume lots of memory. Not acceptable. Generator (iterator) will perform the job much better.
- Note: a generator will provide performance benefits only if we do not intend to use that set of generated values more than once.
- When you use recursion for generator, you need to clear the subgenerator concept. Otherwise, it won’t work as you expect.

### Generator is not pure iterable

Generator is iterator, but it is not pure iterable. That means iter(generator) return itself instead of a fresh iterator.

Basic Example, clone python's built-in range() function:
```py
def my_range(start, stop, step = 1):
    if stop <= start:
        raise RuntimeError("start must be smaller than stop")
    i = start
    while i < stop:
        yield i
        i += step

for k in my_range(10, 50, 3):
    print(k)
```

### Generator functions 

Generator function is defined similar to normal function but there is only one difference, it is using yield keyword to return value used for each iteration.

Any function containing a yield keyword is a generator function; this is detected by Python’s bytecode compiler which compiles the function specially as a result.

Generator function return a lazy iterator. This iterator also call generator object.

### Generator expression(also call generator comprehension)

Generator expressions provide an additional shortcut to build generators out of expressions similar to that of list comprehensions.

If the generated expressions are more complex, involve multiple steps, or depend on additional temporary state, Using generator function .

- generator comprehension

```py
nums_squared_gc = (num**2 for num in range(5))
```
- List comprehension

```py
nums_squared_lc = [num**2 for num in range(5)]
>>>nums_squared_lc
[0, 1, 4, 9, 16]
>>>nums_squared_gc
<generator object <genexpr> at 0x107fbbc78>
```
### The difference 

We can think of list comprehensions as generator expressions wrapped in a list constructor:
```py
# list comprehension
doubles = [2 * n for n in range(50)]
# same as the list comprehension above
doubles = list(2 * n for n in range(50))
```

The difference between generator and list comprehensions is that generator comprehension create lazy generator. it won’t consume all memory it need at one time. But list comprehension will, because list constructor run through the generator.

### Subgenerator (yield from)

- You can use yield from to read data from an other generator.
- There is Subgenerator concept, read more at **PEP380**.

- When you use recursion in generator you will meet some weird issue. That is because you have no subgenerator concept. You need to use yield from for each subgenerator. 
Otherwise you need to use for to run over it (sometime works, sometime not works). Because Generator return Generator Object! it is different from normal function.

### Understand subgenerator

- A Python generator is a form of coroutine, but has the limitation that it can only yield to its immediate caller. This means that a piece of code containing a yield cannot be factored out and put into a separate function in the same way as other code. 

- Performing such a factoring causes the called function to itself become a generator, and it is necessary to explicitly iterate over this second generator and re-yield any values that it produces.

- If yielding of values is the only concern, this can be performed without much difficulty using a loop.

- However, if the subgenerator is to interact properly with the caller in the case of calls to send(), throw() and close(), things become considerably more difficult. As will be seen later, the necessary code is very complicated, and it is tricky to handle all the corner cases correctly.

### Understand `yield from`

- `yield from` is a new syntax. You can check the codes for `yield from` in **PEP380**.
- This new syntax empowers you to refactor generators in a clean way by making it easy to yield every value from an iterator (which a generator conveniently happens to be).
- `yield from` also lets you chain generators together so that values bubble up and down the call stack without code having to do anything special.

- Let's get one thing out of the way first. The explanation that `yield from g` is equivalent to `for v in g: yield v` does not even begin to do justice to what `yield from` is all about. 
- Because, if all `yield from` does is expand the for loop, then it does not warrant adding `yield from` to the language and preclude a whole bunch of new features from being implemented in Python 2.x. PEP380 have the detail.

-- What `yield from` does is it establishes a transparent bidirectional connection between the caller and the sub-generator:
- The connection is "transparent" in the sense that it will propagate everything correctly too, not just the elements being generated (e.g. exceptions are propagated).
- The connection is "bidirectional" in the sense that data can be both sent from and to a generator.

### Example of using recursion in generator: 

You can’t print 0,1,2,3,4 from following code:
```py
def generator_recursion(n):
    if n < 0:
        return
    else:
        generator_recursion(n-1)
        yield n
    
gr = generator_recursion(4)
for k in gr:
   print(k)
```
### You need to use yield from:
```py
def generator_recursion(n):
    if n < 0:
        return
    else:
        yield from generator_recursion(n-1)
        yield n
```

### Passing values into a generator

- In Python 2.5 there’s a simple way to pass values into a generator. 

- yield became an expression, returning a value that can be assigned to a variable or otherwise operated on:
```py
val = (yield i)
val = (yield i) + 10
```

- recommend that you always put parentheses around a yield expression.

- Values are sent into a generator by calling its .send(value) method. This method resumes the generator’s code and the yield expression returns the specified value. 

- Simple example: 
```py
def counter(maximum):
    i = 0
    while i < maximum:
        val = (yield i)
        # If value provided, change counter
        if val is not None:
            i = val
        else:
            i += 1
```
The value of val is always None when regular `__next__()` method is called. When `.send(value)` is called. the val will be the value

### How do `send(value)` resumes the generator?
- When calling `next()`, the code resume from the next line which after last yield. 

- When calling `.send(value)`, the code resume from the last yield! And also yield a value back to `.send(value)` function (like `next()` do).

- You can even can’t send(value) at the very beginning, otherwise get error:
TypeError: can't send non-None value to a just-started generator

## Built-in functions for iterators

- `map()` and `filter()` duplicate the features of generator expressions:
`map(f, iterA, iterB, ...)` returns an iterator over the sequence
`f(iterA[0], iterB[0]), f(iterA[1], iterB[1]), f(iterA[2], iterB[2]), ....`

- `filter(predicate, iter) `
returns an iterator over all the sequence elements that meet a certain condition, and is similarly duplicated by list comprehensions. 

- `enumerate(iter, start=0) `
counts off the elements in the iterable returning 2-tuples containing the count (from start) and each element.
```py
for item in enumerate(['subject', 'verb', 'object']):
    print(item)
(0, 'subject')
(1, 'verb')
(2, 'object')
```
- `Sorted(iterable, key=None, reverse=False)` 
collects all the elements of the iterable into a list, sorts the list, and returns the sorted result. The key and reverse arguments are passed through to the constructed list’s sort() method.

- The `any(iter)` and `all(iter)`
built-ins look at the truth values of an iterable’s contents. `any()` returns True if any element in the iterable is a true value, and `all()` returns True if all of the elements are true values

- `zip(iterA, iterB, ...) `
takes one element from each iterable and returns them in a tuple


## itertools module

The module standardizes a core set of fast, memory efficient tools that are useful by themselves or in combination. Together, they form an “iterator algebra” making it possible to construct specialized tools succinctly and efficiently in pure Python.

### Combinatoric iterators:

1. `permutations(iterable, r=None)`
- Return successive r length permutations of elements in the iterable.
- Return a iterable object, elements are tuple.
- Elements are treated as unique based on their position, not on their value. 
```py
>>> print([p for p in permutations('pro')])
[('p', 'r', 'o'), ('p', 'o', 'r'), ('r', 'p', 'o'), ('r', 'o', 'p'), ('o', 'p', 'r'), ('o', 'r', 'p')]
```

2. `product(*iterables, repeat=1)`
Cartesian product of input iterables.
What Cartesian product do is as following:
`product('ABCD', 'xy') --> Ax Ay Bx By Cx Cy Dx Dy`

repeat parameter
`product(A, repeat=4)` means the same as `product(A, A, A, A)`.
```py
product(range(2), repeat=3) --> 000 001 010 011 100 101 110 111
```

This function is roughly equivalent to the following code, except that the actual implementation does not build up intermediate results in memory:
```py
def product(*args, repeat=1):
    pools = [tuple(pool) for pool in args] * repeat
    result = [[]]
    for pool in pools:
        result = [x+[y] for x in result for y in pool]
    for prod in result:
        yield tuple(prod)
```