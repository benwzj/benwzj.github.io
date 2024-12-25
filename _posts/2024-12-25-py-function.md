---
layout: post
title: Python Function
date: 2024-12-25
categories: Python
tags: Python Function
toc: 
  - name: Function Object
  - name: Function definitions
  - name: Parameter, argument
  - name: Return value
  - name: Lambda
  - name: Scopes and Namespaces
  - name: Code Object
  - name: Closure
  - name: FAQ
---

## Function Object

### Callable Object

#### Callable object including following:
- user-defined functions, 
- built-in functions, 
- methods of built-in objects, 
- class objects, 
- methods of class instances, 
- and all objects having a `__call__()` method are callable! 

#### user-defined functions are callable objects

When you define a function, you are using user-defined function type (function class). Function object is instance of function type. All function type is callable type, support `__call__()`.
You can define your own callable class: 

```bash
>>> class Callable(object):
...     def __init__(self, name):
...         self.name = name
...     def __call__(self, greeting):
...         return '{}, {}!'.format(greeting, self.name)
... 
>>> Callable('World')('Hello')
'Hello, World!'
```

### Function Object features:

- user-defined Function objects are created by function definitions. The only operation on a function object is to call it: `func(argument-list)`.
- Functions are first class objects
In Python, functions behave like any other object, such as an int or a list. That means that you can use functions as arguments to other functions, store functions as dictionary values, or return a function from another function.
- Two different function object types: built-in functions and user-defined functions. 
Both support the same operation (to call the function), but the implementation is different.
- Python creates function objects for you when you use a def statement, or you use a lambda expression:

```bash
>>> def foo(): pass
... 
>>> foo
<function foo at 0x106aafd70>
type(foo)
<class 'function'>
>>> lambda: None
<function <lambda> at 0x106d90668>
```

### user-defined function have some Special attributes: 
- `__name__`, The function’s name.
- `__module__`, The name of the module the function was defined in, or None if unavailable.
- `__code__`, The code object representing the compiled function body.
- `__globals__`, A reference to the dictionary that holds the function’s global variables
- `__dict__`, The namespace supporting arbitrary function attributes.

## Function definitions

A function definition defines a user-defined function object.
```py
funcdef  ::= [decorators] "def" funcname "(" [parameter_list] ")"
               ["->" expression] ":" suite
```

- A function definition is an executable statement. Its execution binds the function name in the current local namespace to a function object (a wrapper around the executable code for the function). This function object contains a reference to the current global namespace as the global namespace to be used when the function is called.
- The function definition does not execute the function body; this gets executed only when the function is called. 

### Decorator 

- Decorators are used to add some design patterns to a function without changing its structure.
- When a function returning another function, usually applied as a function transformation using the @wrapper syntax. This is Decorator. 
- To apply a decorator we first define the decorator function. 
- A function definition may be wrapped by one or more decorator expressions. 
- Common examples for decorators are classmethod() and staticmethod().
- The decorator syntax is merely syntactic sugar, the following two function definitions are semantically equivalent:

```py
def f(arg):
    ...
f = staticmethod(f)
```
and
```py
@staticmethod
def f(arg):
    ...
```

#### Example
we have a decorator function:
```py
def uppercase_decorator(function):
    def wrapper():
        func = function()
        make_uppercase = func.upper()
        return make_uppercase

    return wrapper
```
Option1:
```py
def say_hi():
    return 'hello there'

decorate = uppercase_decorator(say_hi)
print(decorate()) 
#print HELLO THERE
```

Option2:
```py
@uppercase_decorator
def say_hi():
    return 'hello there'

print(say_hi())
#print HELLO THERE
```

### Annotation 

Function annotations are arbitrary python expressions that are associated with various part of functions. 
These expressions are evaluated at compile time and have no life in python’s runtime environment. Python does not attach any meaning to these annotations. 
- Function annotations take life only when interpreted by third party libraries, for example, mypy.
- Python supports dynamic typing and hence no module is provided for type checking. Annotations can provide more information.
- Support simple parameters, excess parameters, nested parameters, return type.
- Type annotations introduced in Python 3.0
- Special attribute `‘__annotations__’` outputs the dictionary having a special key `‘return’` and other keys having name of the annotated arguments.
- standard library pydoc, inspect can provide information of all the annotation of a module.

#### Type hints
Annotations were introduced in Python 3.0 originally without any specific purpose. They were simply a way to associate arbitrary expressions to function arguments and return values.
Years later, PEP 484 defined how to add type hints to your Python code, based off work that Jukka Lehtosalo had done on his Ph.D. project, Mypy.
- Python 3.5 introduced Lib/typing.py (type hints), which you can add to your code using the type annotations introduced in Python 3.0. 
- Now The main purpose of annotation is to add type hints. 
- lib/typing.py — Support for type hints


## Parameter, argument

Parameter refer to reference which appear on the funciton definition.
Argument refer to the value which provided for function when it was bind

### Basic concepts

#### Python supports 5 types of arguments:

- Positional arguments.
- Keyword arguments.
- Any number of positional arguments (*args)
- Any number of keyword arguments (**kwargs)
- Default arguments. 

We could define our own function parameters in a way that restricts them to only accept positional arguments, or only keyword arguments by using the characters / and *. 

#### By default 
a function must be called with the correct number of arguments. (this is different from JavaScript)

#### Positional arguments
Arguments that match up parameters based on their position in the invocation expression are called positional arguments.
- Python will resolve all the arguments to positional arguments. 

#### Keyword Arguments
send arguments with the key = value syntax.
This way the order of the arguments does not matter. (JavaScript is not support this feature)
def my_function(child3, child2, child1):
  print("The youngest child is " + child3)

my_function(child1 = "Emil", child2 = "Tobias", child3 = "Linus")

#### * args
add a * before the parameter name on definition. 
- This way the function will receive a tuple of arguments, and can access the items accordingly by position.
def my_function(*kids):
  print("The youngest child is " + kids[2])

- Parameters after *args are keyword-only. 

####  **kwargs
add two asterisk: ** before the parameter name in the function definition.

This way the function will receive a dictionary of arguments, and can access the items accordingly by key:
```py
def my_function(**kid):
  print("His last name is " + kid["lname"])

my_function(fname = "Tobias", lname = "Refsnes")
```
`**kwargs` must be the last parameter. 

#### Default Parameter Value

When one or more parameters have the form parameter = expression, the function is said to have “default parameter values.”
For a parameter with a default value, the corresponding argument may be omitted from a call, in which case the parameter’s default value is substituted: 
```py
def my_function(country = "Norway"):
  print("I am from " + country)

my_function()
```
If a parameter has a default value, all following parameters up until the “*” must also have a default value — this is a syntactic restriction that is not expressed by the grammar.

Default parameter values are evaluated from left to right when the function definition is executed. This means that the expression is evaluated once, when the function is defined, and that the same “pre-computed” value is used for each call. 
This is especially important to understand when a default parameter value is a mutable object, such as a list or a dictionary. if the function modifies the object, the default parameter value is in effect modified. This is generally not what was intended.
So, DO NOT set mutable type as Default value.

Example:
```py
i = 5

def f(x = i):
    print(x)

i = 6
f() # print 5
```
Another one:
```py
i = [1, 2, 3]

def f(x = i):
    x.append(1)
    print(x)

f()
f()
f([99])
f()

# Print: 
# [1, 2, 3, 1]
# [1, 2, 3, 1, 1]
# [99, 1]
# [1, 2, 3, 1, 1, 1]
```
Try not to set default parameter value as mutable object. Instead, Using following:
```py
def default_parameter_function(lst = none):
  if lst == none:
    lst = []
  # ...
```

### All argument expressions are evaluated before the call is attempted. 
All keyword arguments have to follow all positional arguments. That means all positional arguments provide first, then the keyword argument. 

#### If keyword arguments are present, they are first converted to positional arguments. 

- First, a list of unfilled slots is created for the formal parameters. If there are N positional arguments, they are placed in the first N slots. 
- Next, for each keyword argument, the identifier is used to determine the corresponding slot (if the identifier is the same as the first formal parameter name, the first slot is used, and so on). If the slot is already filled, a TypeError exception is raised. Otherwise, the value of the argument is placed in the slot, filling it (even if the expression is None, it fills the slot). 
- When all arguments have been processed, the slots that are still unfilled are filled with the corresponding default value from the function definition. (Default values are calculated, once, when the function is defined; thus, a mutable object such as a list or dictionary used as default value will be shared by all calls that don’t specify an argument value for the corresponding slot; this should usually be avoided.) 
- If there are any unfilled slots for which no default value is specified, a TypeError exception is raised. Otherwise, the list of filled slots is used as the argument list for the call.

#### *expression argument implementation detail

If the syntax *expression appears in the function call, expression must evaluate to an iterable. Elements from these iterables are treated as if they were additional positional arguments. 
For the call f(x1, x2, *y, x3, x4), if y evaluates to a sequence y1, …, yM, this is equivalent to a call with M+4 positional arguments x1, x2, y1, …, yM, x3, x4.

A consequence of this is that although the *expression syntax may appear after explicit keyword arguments, it is processed before the keyword arguments (and any **expression arguments). So:
```bash
>>> def f(a, b):
...     print(a, b)
...
>>> f(b=1, *(2,))
2 1
>>> f(a=1, *(2,))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() got multiple values for keyword argument 'a'
>>> f(1, *(2,))
1 2
```

#### **expression argument

If the syntax **expression appears in the function call, expression must evaluate to a mapping, the contents of which are treated as additional keyword arguments. 
If a keyword is already present (as an explicit keyword argument, or from another unpacking), a TypeError exception is raised.

### What do single slash /, asterisk * means in parameter.
```py
def func_name(pos_only, /, pos_or_keyword, *, keyword_only):
    # function body
```
- slash `/` denotes that the parameters prior to it are positional-only.
- asterisk * denotes that after *, it accept more optional arguments which must be specified as keyword arguments, but no positional arguments is accepted. 
(*args have same effect as *, it accept keyword arguments only after *args)
- e.g. built-in sorted function:
`sorted(iterable, /, *, key=None, reverse=False)`
- e.g. sum built-in function:`sum(iterable, /, start=0)`. sum accept up to 2 arguments, the second one don’t have to be keyword argument. 


## Return value
A call always returns some value, possibly None, unless it raises an exception. How this value is computed depends on the type of the callable object.
If it is
- a user-defined function:
The code block for the function is executed, passing it the argument list. The first thing the code block will do is bind the formal parameters to the arguments; this is described in section Function definitions. When the code block executes a return statement, this specifies the return value of the function call.
- a built-in function or method:
The result is up to the interpreter; see Built-in Functions for the descriptions of built-in functions and methods.
- a class object:
A new instance of that class is returned.
- a class instance method:
The corresponding user-defined function is called, with an argument list that is one longer than the argument list of the call: the instance becomes the first argument.
- a class instance:
The class must define a `__call__()` method; the effect is then the same as if that method was called.


## Lambda

A lambda function is a small anonymous function.
A lambda function can take any number of arguments, but can only have one expression.

The expression lambda parameters: expression yields a function object. The unnamed object behaves like a function object defined with:
```py
def <lambda>(parameters):
    return expression
```
Note that functions created with lambda expressions cannot contain statements or annotations.

### Name of lambda
```py
f = lambda x: x
print(f.__code__.co_name) # <lambda>
```
The word `'<lambda>'` can't be the name of a normal function in Python, since it contains two invalid characters: < and >.

### Why lambda
The power of lambda is better shown when you use them as an anonymous function inside another function.

### How to use lambda
don’t need return keyword. 

Just like curry function in JavaScript: 
```py
def myfunc(n):
  return lambda a : a * n

mydoubler = myfunc(2)
mytripler = myfunc(3)

print(mydoubler(11))
print(mytripler(11))
```

Or same as high order function:
```bash
>>> student_tuples = [
...     ('john', 'A', 15),
...     ('jane', 'B', 12),
...     ('dave', 'B', 10),
... ]
>>> sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

## Scopes and Namespaces

Namespace is the mapping from names to objects.
Scope is the available variables in the specific area.

### Namespaces
- A namespace is a mapping from names to objects. 
- In a sense the set of attributes of an object also form a namespace. 

Namespaces are created at different moments and have different lifetimes:
- The namespace containing the built-in names is created when the Python interpreter starts up, and is never deleted. 
- The global namespace for a module is created when the module definition is read in; normally, module namespaces also last until the interpreter quits.
- The local namespace for a function is created when the function is called, and deleted when the function returns or raises an exception that is not handled within the function. (Of course, recursive invocations each have their own local namespace.)

### Scopes

#### What is Scope
- A scope is a region of code where a given set of name bindings is accessible.
- A scope can be thought of as an area in a script where given variables are available.
- A scope is a textual region of a Python program where a namespace is directly accessible. “Directly accessible” here means that an unqualified reference to a name attempts to find the name in the namespace.
- Although scopes are determined statically, they are used dynamically. 

#### How Scope works 

Python follows the LEGB rule to determine what value to resolve a variable with when it's encountered, or whether to throw an error.

LEGB stands for Local, Enclosing, Global and Built-in.
This rule simply states the order of scopes in which Python searches for the existence of a given variable.

At any time during execution, there are 3 or 4 nested scopes whose namespaces are directly accessible:
- the innermost scope, which is searched first, contains the local names
- the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contains non-local, but also non-global names
- the next-to-last scope contains the current module’s global names
- the outermost scope (searched last) is the namespace containing built-in names

`global` or `nonlocal` statement affect the Scopes
- The global statement can be used to indicate that particular variables live in the global scope and should be rebound there; 
- The nonlocal statement indicates that particular variables live in an enclosing scope and should be rebound there.

### Example
```py
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```
The output of the example code is:
```
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```

### Python is a lexically-scoped language

(Lexically-scoped also known as statically-scoped. )
Many modern programming languages including C, Python, JavaScript, etc. are lexically-scoped languages. They work on a lexical-scoping model.

Lexical-scoping implies the fact that names are accessible only where they are defined, not anywhere else.

It means that a name is available only in its lexical context i.e the place where it is defined in the source code, not in the calling context (also known as dynamic context).
```py
x = 10

def f1():
    print(x)

def f2():
    x = 20
    f1()

f2()# 10
```
The word 'lexical' means 'source code'

#### What exactly is meant by the term 'context'.

At any given point in a piece of code, the set of all names available is known as a context.
A context is sometimes also referred to as an environment.

lexical context example: 
```py
# outer lexical context of f()
a = 10
b = 20

def f():
    # local lexical context
    c = 30
    d = 40
```
In contrast, a dynamic context, or an execution context, depends upon the state of a program i.e a particular stage of its execution.


## Code Object

### What is Code object
Code objects are a low-level detail of the CPython implementation. Each one represents a chunk of executable code that hasn’t yet been bound into a function. Code objects  represent byte-compiled executable Python code, or bytecode.

#### The difference between a code object and a function object 
is that the function object contains an explicit reference to the function’s globals (the module in which it was defined), while a code object contains no context; also the default argument values are stored in the function object, not in the code object (because they represent values calculated at run-time). Unlike function objects, code objects are immutable and contain no references (directly or indirectly) to mutable objects.

#### built-in function `compile()` return code object.
example:
```py
code_str = """
print("Hello Code Objects")
"""
# Create the code object
code_obj = compile(code_str, '<string>', 'exec')
# get the code object
print(code_obj)
#Attributes of code object
print(dir(code_obj))
# The filename
print(code_obj.co_filename)
# The first chunk of raw bytecode
print(code_obj.co_code)
#The variable Names
print(code_obj.co_varnames)
```

### What is Code object of function
A code object describes different aspects of a compiled function, based on its byte code.

- They expose various details about a function such as the number of positional arguments it must have; the number of its local variables; its free variables;
- Every function in Python has a `__code__` attribute that holds its code object.

### Some attributes of code object.
`co_nlocals` — is the number of local variables used by the function (including arguments).
`co_argcount` — is the total number of positional arguments (including positional-only arguments and arguments with default values).
`co_varnames` — is a tuple containing the names of the local variables (starting with the argument names).
`co_names` — is a tuple containing the names used by the bytecode.
`co_cellvars` — is a tuple containing the names of local variables that are referenced by nested functions.
`co_freevars` — is a tuple containing the names of free variables; co_code is a string representing the sequence of bytecode instructions.

#### Free variable
Any non-local, non-global, variable used by a function is referred to as a free variable of the function.
x is free variable for bar function at following:
```py
def foo():
    x = 10

    def bar():
        print(x)
    return bar

bar_fun = foo()
print(bar_fun.__code__.co_freevars) #('x',)
```

#### cell variable
For a given function, a cell variable is a local variable that is used by a nested function.

A cell variable is an idea related to that of a free variable. If a variable x is a free variable for a function bar then that same variable would be a cell variable for a function foo:
```py
def foo():
    x = 10

    def bar():
        print(x)
    return bar

bar_fun = foo()
print(foo.__code__.co_cellvars) #('x',)
```

## Closure

### What is closure
A function along with the environment of its enclosing function, are collectively referred to as a closure.
(The concept of closure is same with JavaScript. But the implementation should be different)

- We can nest functions within functions in Python. Moreover, the inner function might additionally use data defined inside the outer function. 
- A function returned from another function that uses one of its local variables, is a closure.
- But Essentially, any function that captures its enclosing environment inside its `__closure__` attribute is a closure. It don’t have to be returned function (that guy said so).

### How to implement closure

#### It is important to understand cell variable and free variable
- A local variable is referred to as a cell variable of the function if it's used by an inner (nested) function.
A cell variable, therefore, is simply a variable used in multiple scopes.
- A free variable is a variable not defined in the local scope of the function, neither a parameter, nor a global.
a free variable is a non-local variable of a function, defined in an outer function.

#### co_freevars attribute
For a given function, its code object contains a co_freevars attribute that contains the names of all the free variables of the function. 

#### `__closure__` attribute
Its `__closure__` attribute contains a tuple holding the values of each of its free variables; or else the value None.
the co_freevars attribute of the function's code object holds the names of all free variables, while the `__closure__` attribute holds their values. And these valueis cell object: 
```py
def foo():
    a = 10
    b = 20

    def bar():
        print(a, b)
    
    return bar

bar = foo()
bar.__closure__
(<cell at 0x7f9d64c68460: int object at 0x954f40>, <cell at 0x7f9d643a4eb0: int object at 0x955080>)
```
`__closure__` attribute holds tuple of cell objects — each corresponding to a free variable.

#### What is cell object

“Cell” objects are used to implement variables referenced by multiple scopes. For each such variable, a cell object is created to store the value; the local variables of each stack frame that references the value contains a reference to the cells from outer scopes which also use that variable. 

When the value is accessed, the value contained in the cell is used instead of the cell object itself. This de-referencing of the cell object requires support from the generated byte-code; these are not automatically de-referenced when accessed. 

Cell objects are not likely to be useful elsewhere.


## FAQ

- The difference between namespace and scope concept
- What is the class for Function object?

### What is the relationship between function and method
Methods are functions that are called using the attribute notation.
- If you access a method (a function defined in a class namespace) through an instance, you get a special object: a bound method object. 
- When method is called, it will add the self argument to the argument list. Bound methods have two special read-only attributes: `m.__self__` is the object on which the method operates, and `m.__func__` is the function implementing the method. Calling m(arg-1, arg-2, ..., arg-n) is completely equivalent to calling `m.__func__`(`m.__self__`, arg-1, arg-2, ..., arg-n).

### What is Code Objects?

Code objects are returned by the built-in compile() function and can be extracted from function objects through their `__code__` attribute. 

Code objects are used by the implementation to represent “pseudo-compiled” executable Python code such as a function body. They differ from function objects because they don’t contain a reference to their global execution environment.

### What is happening for the following code?
```py
def f(x = []):
    x.append(1)
    return x

print f()
print f()
print f()
print f(x = [9,9,9])
print f()
print f()
[1]
[1, 1]
[1, 1, 1]
[9, 9, 9, 1]
[1, 1, 1, 1]
[1, 1, 1, 1, 1]
```

Binding of default arguments occurs at function definition. 
Default parameter values are evaluated from left to right when the function definition is executed. This means that the expression is evaluated once, when the function is defined, and that the same “pre-computed” value is used for each call. 
This is especially important to understand when a default parameter value is a mutable object, such as a list or a dictionary: if the function modifies the object (e.g. by appending an item to a list), the default parameter value is in effect modified. 
This is generally not what was intended. A way around this is to use None as the default, and explicitly test for it in the body of the function, e.g.:
```py
def whats_on_the_telly(penguin=None):
    if penguin is None:
        penguin = []
    penguin.append("property of the zoo")
    return penguin
```

### What's Type Hinting
Type hinting in Python is basically declaring that the parameters in your functions and methods have a certain type. 
example: 
Normal function: 
```py
def add(a, b):
    return a + b
```
function with type hinting:
```py
def add(a: int, b: int) -> int:
    return a + b

if __name__ == '__main__':
    print(add(2, 4))
```

### What is Variable Annotation
Let's say you want to not just annotate function parameters but also regular variables. 
from typing import List
```py
def odd_numbers(numbers: List) -> List:
    odd: List[int] = [] # this is variable annotation
    for number in numbers:
        if number % 2:
            odd.append(number)

    return odd
```

### nonlocal concept

#### In Python, 
The nonlocal statement causes the listed identifiers to refer to previously bound variables in the nearest enclosing scope excluding globals. 
This is important because the default behavior for binding is to search the local namespace first. 
The nonlocal statement allows encapsulated code to rebind variables outside of the local scope besides the global (module) scope.

Following is a nested function inner defined in the scope of another function outer. The variable x is local to outer, but non-local to inner (nor is it global):
```py
def outer():
    x = 1
    def inner():
        nonlocal x
        x += 1
        print(x)
    return inner
```
#### In Javascript
In Javascript, the locality of a variable is determined by the closest var statement for this variable. In the following example, x is local to outer as it contains a var x statement, while inner doesn't. Therefore, x is non-local to inner:
```js
function outer() {
    var x = 1;
    function inner() {
        x += 1;
        console.log(x);
    }
    return inner;
}
```
