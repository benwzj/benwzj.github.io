---
layout: post
title: Python Data Model Overview
date: 2024-12-12
categories: Python
tags: Python
toc:
  - name: Object
  - name: Data Type
    subsections: 
      - name: None, NotImplemented, Ellipsis
      - name: Numbers module
      - name: Sequences types
      - name: Set types
      - name: Mappings
      - name: Callable types
      - name: Modules
      - name: Custom classes
      - name: Class instances
      - name: I/O objects 
---

## Object

Objects are Python’s abstraction for data. All data in a Python program is represented by objects or by relations between objects. (Everything in python is object)
Every object has an identity, a type and a value. 

### Every object has an identity, a type and a value.
- An object’s identity never changes once it has been created; you may think of it as the object’s address in memory. 
  - The `is` operator compares the identity of two objects; 
  - the `id()` function returns an integer representing its identity.
- An object’s type determines the operations that the object supports (e.g., “does it have a length?”) and also defines the possible values for objects of that type. 
  - The `type()` function returns an object’s type (which is an object itself). 
  - Like its identity, an object’s type is also unchangeable.
- An object’s mutability is determined by its type; for instance, numbers, strings and tuples are immutable, while dictionaries and lists are mutable.

### Container
Some objects contain references to other objects; these are called containers. Examples of containers are tuples, lists and dictionaries. The references are part of a container’s value. 

## Data Type

### None, NotImplemented, Ellipsis
The following data type have a single value. There is a single object with this value, And These objects are accessed through a built-in name None, NotImplemented, Ellipsis (or ...):
None, NotImplemented, Ellipsis 

### Numbers module
The numbers module defines a hierarchy of numeric abstract base classes which progressively define more operations. None of the types defined in this module are intended to be instantiated.
#### class numbers.Number
The root of the numeric hierarchy. If you just want to check if an argument x is a number, without caring what kind, use isinstance(x, Number).
#### class numbers.Integral 
There are two types of integers:
- Integers (int) - These represent numbers in an unlimited range, subject to available (virtual) memory only. It is a whole number, positive or negative, without decimals, of unlimited length.
- Booleans (bool) - The two objects representing the values False and True are the only Boolean objects. (False and True are keyword in Python)
#### numbers.Real (float)
Float represent machine-level double precision floating point numbers. You are at the mercy of the underlying machine architecture (and C or Java implementation) for the accepted range and handling of overflow.
#### numbers.Complex (complex)

### Sequences types
- Sequences represent finite ordered sets indexed by non-negative numbers.
- Sequences are distinguished according to their mutability:
Immutable sequences, like Strings, Bytes, Tuples, 
Mutable sequences, like Lists, Byte Arrays. 

#### Common Sequences operations
- The built-in function `len()` returns the number of items of a sequence. 
- Item i of sequence a is selected by `a[i]`
- Sequences also support slicing: `a[i:j:step]`
- When looping through a sequence, the position index and corresponding value can be retrieved at the same time using the `enumerate()` function.

### Set types
These represent unordered, finite sets of unique, immutable(we can’t change the content of items, but you can add or remove items) objects.
There are currently two intrinsic set types:
- Sets
These represent a mutable set. They are created by the built-in set() constructor and can be modified afterwards by several methods, such as add().
- Frozen sets
These represent an immutable set. They are created by the built-in frozenset() constructor. As a frozenset is immutable and hashable, it can be used again as an element of another set, or as a dictionary key.

### Mappings
These represent finite sets of objects indexed by arbitrary index sets. The subscript notation `a[k]` selects the item indexed by k from the mapping a; this can be used in expressions and as the target of assignments or del statements. The built-in function `len()` returns the number of items in a mapping.

#### Dictionary
- Dictionaries are used to store data values in key:value pairs.
- A dictionary is a collection which is ordered, changeable and not allow duplicates.
- Dictionaries are written with curly brackets, and have keys and values
- The only types of values not acceptable as keys are values containing lists or dictionaries or other mutable types that are compared by value rather than by object identity, the reason being that the efficient implementation of dictionaries requires a key’s hash value to remain constant. 

### Callable types

These are the data types to which the function call operation can be applied:

#### User-defined functions
A user-defined function object is created by a function definition. 
- It’s Special attributes: like `__name__`, `__module__`, `__defaults__`, `__code__`, `__closure__`, etc. 
`__code__` refer to The code object representing the compiled function body.
- Function objects also support getting and setting arbitrary attributes, which can be used, for example, to attach metadata to functions. 
- Lambda: A lambda function is a small anonymous function.
A lambda function can take any number of arguments, but can only have one expression.

#### Instance methods
An instance method object combines a class, a class instance and any callable object (normally a user-defined function).

#### Generator functions
A function or method which uses the yield statement (see section The yield statement) is called a generator function. Such a function, when called, always returns an iterator object which can be used to execute the body of the function: calling the iterator’s iterator`.__next__()` method will cause the function to execute until it provides a value using the yield statement. When the function executes a return statement or falls off the end, a StopIteration exception is raised and the iterator will have reached the end of the set of values to be returned. 

#### Coroutine functions
A function or method which is defined using async def is called a coroutine function. Such a function, when called, returns a coroutine object. It may contain await expressions, as well as async with and async for statements. 

#### Asynchronous generator functions
A function or method which is defined using async def and which uses the yield statement is called a asynchronous generator function. Such a function, when called, returns an asynchronous iterator object which can be used in an async for statement to execute the body of the function.

#### Built-in functions
A built-in function object is a wrapper around a C function. Examples of built-in functions are len() and math.sin() (math is a standard built-in module). The number and type of the arguments are determined by the C function. Special read-only attributes: `__doc__` is the function’s documentation string, or None if unavailable; `__name__` is the function’s name; `__self__` is set to None (but see the next item); `__module__` is the name of the module the function was defined in or None if unavailable.

#### Built-in methods
This is really a different disguise of a built-in function, this time containing an object passed to the C function as an implicit extra argument. An example of a built-in method is alist.append(), assuming alist is a list object. In this case, the special read-only attribute `__self__` is set to the object denoted by alist.

#### Classes
Classes are callable. These objects normally act as factories for new instances of themselves, but variations are possible for class types that override `__new__()`. The arguments of the call are passed to `__new__()` and, in the typical case, to `__init__()` to initialize the new instance.

#### Class Instances
Instances of arbitrary classes can be made callable by defining a `__call__()` method in their class.

### Modules
Modules are a basic organizational unit of Python code, and are created by the import system as invoked either by the import statement, or by calling functions such as importlib.`import_module()` and built-in `__import__()`.

- A module object has a namespace implemented by a dictionary object (this is the dictionary referenced by the `__globals__` attribute of functions defined in the module). Attribute references are translated to lookups in this dictionary, e.g., m.x is equivalent to `m.__dict__["x"]`. 
- A module object does not contain the code object used to initialize the module.
- To create a module just save the code you want in a file with the file extension .py.

#### Datetime, Math, JSON, RegEx, os, etc. are built-in modules. 
For example, you want to handle files in your hard disk. You can use os modules which provide functions to help you out. 

#### PIP
PIP is a package manager for Python packages. Just like npm for JavaScript. You can use pip to instal third party packages, like django, etc.
- A package contains all the files you need for a module.
- Modules are Python code libraries you can include in your project.

For example: 
openpyxl help you to handle excel file:
`$pip3 install openpyxl`

Use the list command to list all the packages installed on your system:
`$pip3 list`

### Custom classes
Custom class types are typically created by class definitions.

Basic class Example:
```py
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

  def myfunc(self):
    print("Hello my name is " + self.name + ", age is " + str(self.age))

p1 = Person("John", 36)
p1.myfunc()
```

#### __init__() Function
All classes have a function called `__init__()`, which is always executed when the class is being initiated.
- The self parameter is a reference to the current instance of the class, and is used to access variables that belong to the class.
- Can change property value, can delete property.

#### Inheritance
```py
class Student(Person):
  def __init__(self, fname, lname, year):
    super().__init__(fname, lname)
    self.graduationyear = year

  def welcome(self):
    print("Welcome", self.firstname, self.lastname, "to the class of", self.graduationyear)
```

`super()` function will make the child class inherit all the methods and properties from its parent.

### Class instances
A class instance is created by calling a class object.

### I/O objects 
(also known as file objects)
- A file object represents an open file. Various shortcuts are available to create file objects: the open() built-in function, and also os.popen(), os.fdopen(), and the makefile() method of socket objects 
- The objects sys.stdin, sys.stdout and sys.stderr are initialized to file objects corresponding to the interpreter’s standard input, output and error streams; they are all open in text mode and therefore follow the interface defined by the io.TextIOBase abstract class.

