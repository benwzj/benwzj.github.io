---
layout: post
title: Python Build-in Type
date: 2024-12-11
categories: Python
tags: Python
---

JavaScript have primitive type concept. But Python don’t.


Data type in python: 
- Numeric, 
- Sequence,
- Set,
- Mapping.

(String, list, tuple, range and byte are sequence. )

--- Numeric

--- int
integer is a whole number, positive or negative, without decimals, of unlimited length.

--- float
Float, or "floating point number" is a number, positive or negative, containing one or more decimals.

--- Complex
Complex numbers are written with a "j" as the imaginary part

--- Questions

--- What is float('inf')

- It acts as an unbounded upper value for comparison. 
- This is useful for finding lowest values for something. 

-- for example, calculating path route costs when traversing trees.
>>> lowest_path_cost = float('inf')
>>> # pretend that these were calculated using some worthwhile algorithm
>>> path_costs = [1, 100, 2000000000000, 50]
>>> for path in path_costs:
...   if path < lowest_path_cost:
...     lowest_path_cost = path
...
>>> lowest_path_cost

- if you didn't have float('Inf') available to you, what value would you use for the initial lowest_path_cost? Would 9999999 be enough -- float('Inf') removes this guesswork.

--- Binary Sequence

The core built-in types for manipulating binary data are bytes and bytearray. They are supported by memoryview which uses the buffer protocol to access the memory of other binary objects without needing to make a copy.

--- Bytes Objects
Firstly, the syntax for bytes literals is largely the same as that for string literals, except that a b prefix is added:
Single quotes: b'still allows embedded "double" quotes'

Only ASCII characters are permitted in bytes literals (regardless of the declared source code encoding). Any binary values over 127 must be entered into bytes literals using the appropriate escape sequence.

bytes object share all behavior with bytearray except for it’s immutability.

-- In addition to the literal forms, bytes objects can be created in a number of other ways:
- A zero-filled bytes object of a specified length: bytes(10)
- From an iterable of integers: bytes(range(20))
- Copying existing binary data via the buffer protocol: bytes(obj)

-- classmethod fromhex(string)
This bytes class method returns a bytes object, decoding the given string object. The string must contain two hexadecimal digits per byte, with ASCII whitespace being ignored.
>>> bytes.fromhex('2Ef0 F1f2  ')
b'.\xf0\xf1\xf2'

-- hex()
Return a string object containing two hexadecimal digits for each byte in the instance.
>>> b'\xf0\xf1\xf2'.hex()
'f0f1f2'

--- Bytearray Objects

There is no dedicated literal syntax for bytearray objects, instead they are always created by calling the constructor:
- Creating an empty instance: bytearray()
- Creating a zero-filled instance with a given length: bytearray(10)
- From an iterable of integers: bytearray(range(20))
- Copying existing binary data via the buffer protocol: bytearray(b'Hi!')

-- support classmethod fromhex(string) and hex() as well. akin to bytes. 

--- String

Textual data in Python is handled with str objects, or call strings. Strings are immutable sequences of Unicode code points.

- Like JavaScript, strings in Python are arrays of bytes representing unicode characters.
- Square brackets can be used to access elements of the string.
- You can loop through a string.
- Using len() to get length of a string.
- Support keyword in, not in.
- Python doesn’t have character type, a single character is simply a string with a length of 1.
- You can assign a multiline string to a variable by using three quotes.
- Python has a set of built-in methods that you can use on strings.

--- Basic String common operation

-- String slice syntax
string slice is a convenient way to operate string.

b = "Hello, World!"
b[2]    #"l"
b[2:5]  #"llo"
b[::-1]  #reverse a string: "!dlroW ,olleH"

-- split string
a = "Hello, World!"
print(a.split(",")) # ['Hello', ' World!']

-- join string
opposite of split(), joins the elements in the given list together.
a = ['Hello', ' World!']
print(",".join(a)) # "Hello, World!"

-- replace 
a = "Hello, World!"
print(a.replace("H", "J")) # "Jello, World!"

-- find() and rfind()
find() return the index of the first location.
rfine() return the index of last one.

-- s.lower(), s.upper() 
returns the lowercase or uppercase version of the string

-- translate()
The translate() method returns a string where some specified characters are replaced with the character described in a dictionary, or in a mapping table.

- Use the str.maketrans() method to create a mapping table.

--- str Object

class str(object='')
class str(object=b'', encoding='utf-8', errors='strict')

Return a string version of object.
If object is not provided, returns the empty string. Otherwise, the behavior of str() depends on whether encoding or errors is given. 

print("20 days are " + str(20*24*60*60) +" seconds")

--- String module

string module is a built-in module and we have to import it before using any of its constants and classes.
Because str class, String module have been old. and you should not use.

--- String format

There are several way to format, manipulate string:
- printf-style, template, str.format() have been old style way.
- f-string is the latest way and recommend use.

-- printf-style String Formatting (old sytle)

String objects have one unique built-in operation: the % operator (modulo). 
The effect is similar to using the sprintf() in the C language.
- Example: 
print('%(language)s has %(number)03d quote types.' %
      {'language': "Python", "number": 2})
// Python has 002 quote types.

def dec2hexstring(dec):
    decimal = int(dec)
    hexa = '%#X' % decimal
    return hexa

- But recommend to use the newer formatted string literals, the str.format() interface, or template strings. f-String is latest way to format string!

-- str.format(*args, **kwargs)

Perform a string formatting operation. (Format string means create string, make string.)
The string on which this method is called can contain literal text or replacement fields delimited by braces {}. 

- Introduced in Python 2.6.
- str.format(), when deal with multiple arguments, and long strings. it will looks verbose.

>>> "The sum of 1 + 2 is {0}".format(1+2)
'The sum of 1 + 2 is 3'

>>> '{2}, {1}, {0}'.format('a', 'b', 'c')
'c, b, a'

- Format String Syntax:
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"

• field_name can be 0, 1, 2... which is positional of the parameters.
or be nothing, which still positional.
or be key name of the dic which is the parameter.
or be attribute of object which is the parameter.

• conversion have three options: !s, !r, !a
>>> "repr() shows quotes: {!r}; str() doesn't: {!s}".format('test1', 'test2')
"repr() shows quotes: 'test1'; str() doesn't: test2"

• format_spec is a Mini-Languag! Start from ‘:’. For example display in Hexadecimal, or octal, binary. It can do more, specify the type, fill, align, width, precision :
>>> for align, text in zip('<^>', ['left', 'center', 'right']):
...     '{0:{fill}{align}16}'.format(text, fill=align, align=align)
...
'left<<<<<<<<<<<<'
'^^^^^center^^^^^'
'>>>>>>>>>>>right'
Using the comma as a thousands separator:
>>> '{:,}'.format(1234567890)
'1,234,567,890'

- Nesting arguments:
for align, text in zip('<^>', ['left', 'center', 'right']):
print('{:{fill}{align}30}'.format(text, fill=align, align=align))

-- Template strings

Template strings is very simple string formatting tool which focus on substitutions.

- A primary use case for template strings is for internationalization (i18n) since in that context, the simpler syntax and functionality makes it easier to translate than other built-in string formatting facilities in Python. 

- The string module provides a Template class that implements these rules.

- Main methods is substitute(), and safe_substitute()

from string import Template
s = Template('$who likes $what')
s1 = s.substitute(who='tim', what='kung pao')
print(s1) #'tim likes kung pao'

-- formatted string literal / f-Strings (Python 3.6+ new syntax)

- (f-String are similar to JavaScript’s Template Literal)
- A formatted string literal or f-string is a string literal that is prefixed with 'f' or 'F'. 
- These strings may contain replacement fields, which are expressions delimited by curly braces {}. 
- While other string literals always have a constant value, formatted strings are really expressions evaluated at run time.
- f-strings are a great new way to format strings. Not only are they more readable, more concise, and less prone to error than other ways of formatting, but they are also faster!
- format syntax is similar to str.format()

>>> name = "Fred"
>>> f"He said his name is {name!r}."
"He said his name is 'Fred'."
>>> f"He said his name is {name}."
"He said his name is Fred."

>>> number = 1024
>>> f"{number:#0x}"  # using integer format specifier
'0x400'

- Support triple quote mark. So it is easy to support Multiline f-Strings

- f-string support function
>>> def to_lowercase(input):
...     return input.lower()

>>> name = "Eric Idle"
>>> f"{to_lowercase(name)} is funny."
'eric idle is funny.'


--- Unicode Support

Great presentation about Unicode:
https://nedbatchelder.com/text/unipain.html

-- Overview

- Strings can either be represented in bytes or unicode code points.
Byte strings and unicode strings each have a method to convert it to the other type of string. 
unicode .encode() → bytes
bytes .decode() → unicode

- In Python 2, a string is by default a binary string and you need to use u'' to mark a string as a Unicode string. 
If you try to perform a string operation that combines a unicode string with a byte string, Python 2 try to be helpful, it will implicitly decode the byte string to produce a second unicode string, then complete the operation.
But the conversion from int to float can’t fail, while byte string to unicode string can.

- In Python 3, a string by default is a Unicode string (You don’t need to know HOW Python deal with it), and you need to use b'' to explicitly mark a string as a binary string.
bytes and str are different types. In Python3, they are not converted to each other implicitly.  You need to handle both types by yourself.

- The default encoding for Python source code is UTF-8, so you can simply include a Unicode character (or use u escape) in a string literal:
print("Fichier non trouvé")
>>> "\u0394"     # Using a 16-bit hex value
'\u0394'
Python 3 also supports using Unicode characters in identifiers. (JavaScript support as well)

-- Unicode Sandwich theory

- A good practice is to decode your bytes in UTF-8 (or an encoder that was used to create those bytes) as soon as they are loaded from a file. Run your processing on unicode code points through your Python code, and then write back into bytes into a file using UTF-8 encoder in the end. 

- Python3 handles Unicode strings with unicode code points. You don’t need to know HOW. This is the job of Python 3. You can just take it as a black box. Python3 make sure you can use all the methods of str type. Just remember the Sandwich mode! What you need to handle is decode as soon as possible, and encode at the end. 

-- 5 fact of life about unicode string in Python

1, everything in a computer is bytes. 
Files on disk are a series of bytes, and network connections only transmit bytes. 

2, The world needs more than 256 symbols

3, Bytes and Unicode, Need to keep them straight, Need to deal with both.
- You can’t pretend that everything is bytes, or everything is unicode. You need to use each for their purpose, and explicitly convert between them as needed.
- So Python 2’s pain is deferred: you think your program is correct, and find out later that it fails with exotic characters.
- With Python 3, your code fails immediately, so even if you are only handling ASCII, you have to explicitly deal with the difference between bytes and unicode.

4, Encoding is out-of-band. You cannot infer the encoding of bytes. You must be told, or you have to guess.

5, Data is dirty. Sometimes you are told wrong.

-- 3 Pro tips
- Unicode Sandwich
- Know what you have! Bytes or Unicode? If bytes, what encoding?
- Test Unicode

-- Reading files

In Python 3, the two modes produce different results. When you open a file in text mode, either with “r”, or by defaulting the mode entirely, the data read from the file is implicitly decoded into Unicode, and you get str objects.
If you open a file in binary mode, by supplying “rb” as the mode, then the data read from the file is bytes, with no processing done on them.
>>> open("hello.txt", "r").read()
'Hello, world!\n'
>>> open("hello.txt", "rb").read()
b'Hello, world!\n'

- To get the file read properly, you should specify an encoding to use. The open() function now has an optional encoding parameter.
>>> open("hi_utf8.txt", "r", encoding="utf-8").read()
'Hi \u2119\u01b4\u2602\u210c\xf8\u1f24'
>>> open("hi_utf8.txt", "r").read() # wrong encoding
'Hi \xe2\u201e\u2122\xc6\xb4\xe2\u02dc\u201a\xe2\u201e\u0152\xc3\xb8\xe1\xbc\xa4'

-- Internal representations for Unicode strings

Even the default encoding for Python source code is UTF-8. But for Unicode strings, Python uses three kinds of internal representations :
1 byte per char (Latin-1 encoding)
2 bytes per char (UCS-2 encoding)
4 bytes per char (UCS-4 encoding)

If all characters in a string can fit in ASCII range, then they are encoded using 1-byte Latin-1 encoding. But if there are any char which need two bytes, then Python will encoding all to UCS-2. same

- This is because each character in a string must take up an equivalent number of bytes! 
otherwise, operations such as indexing, slicing would be inaccurate. 
s0 = 'a'
print(sys.getsizeof(s0+'b') - sys.getsizeof(s0))
>1
s1 = '你'
print(sys.getsizeof(s1+'你') - sys.getsizeof(s1))
>2
print(sys.getsizeof(s1+'你a') - sys.getsizeof(s1))
>4
s1 = '🐍'
print(sys.getsizeof(s1+'🐍你') - sys.getsizeof(s1))
>8

- Within CPython, unicode characters are stored as PyUnicodeObject instances. We can view the format of a PyUnicodeObject by looking at the source code:


-- build-in function chr(), ord()

Convert between One-character Unicode strings and code point value.
These tow functions do nothing about encoding. 

- chr() takes code point value(integers) and returns a Unicode string of length 1 that contains the corresponding code point. 
- ord() takes a one-character Unicode string and returns the code point value (integers). 
# '\u0041' == 'A'
>>> chr(65)
'\u0041'
>>> ord('\u0041')
65

-- Unicode Literals in Python Source Code
In Python source code, specific Unicode code points can be written using the \u escape sequence, which is followed by four hex digits giving the code point. The \U escape sequence is similar, but expects eight hex digits, not four:

>>> s = "a\xac\u1234\u20ac\U00008000"
>>> [ord(c) for c in s]
[97, 172, 4660, 8364, 32768]

-- bytes.decode() and str.encode()

- bytes.decode() can create a string. 
This method takes an encoding argument, such as UTF-8, and optionally an errors argument. (\xNN escape sequence means that NN is Hexadecimal value)
>>> b'\x41'.decode("utf-8")
A
>>> b'\x41\x00'.decode("utf-16")
A

- str.encode(), returns a bytes representation of the Unicode string, encoded in the requested encoding. (opposite method of bytes.decode())
>>> u = chr(65) + 'abcd' + chr(66)
>>> u.encode('utf-8')
b'AabcdB'
>>> u.encode('utf-16')
b'\xff\xfeA\x00a\x00b\x00c\x00d\x00B\x00'

- Unicode code point for emoji 🖐 is U+1F590

print(("你好🖐").encode("unicode_escape"))
b'\\u4f60\\u597d\\U0001f590'

print(("你好🖐").encode()) 
b'\xe4\xbd\xa0\xe5\xa5\xbd\xf0\x9f\x96\x90'

-- codecs module
The low-level routines for registering and accessing the available encodings are found in the codecs module. Implementing new encodings also requires understanding the codecs module. 


-- Uncode in JavaScript

- While a JavaScript source file can have any kind of encoding, JavaScript will then convert it internally to UTF-16 before executing it.
- JavaScript strings are all UTF-16 sequences, as the ECMAScript standard says:
When a String contains actual textual data, each element is considered to be a single UTF-16 code unit.
- UTF-16 encoding need to use with surrogate pairs. For the code points which bigger than U+FFFF. 
- Example (use u escape):
console.log('the heart eyes emoji is \ud83d\ude0d')
> the heart eyes emoji is 😍

-- Tips for Writing Unicode-aware Programs
- Software should only work with Unicode strings internally, 
- decoding the input data as soon as possible and, 
- encoding the output only at the end.

--- String interning

String interning is a method of storing only one copy of each distinct string value in memory, which must be immutable.

- String interning concept is akin to shared object concept! shared object is more popular in immutable object.

- String Interning can Saving Memory, Fast Comparisons, Fast Dictionary Lookups.

-- Implicit String interning. 
- Implicitly intern string depends on several factors:
• All empty strings and strings of length 1 are interned.
• Up until version 3.7, Python used peephole optimization, and all strings longer than 20 characters were not interned. However, now it uses the AST optimizer, and (most) strings up to 4096 characters are interned.
• Names of functions, class, variables, arguments, etc. are implicitly interned.
• The keys of dictionaries used to hold module, class, or instance attributes are interned.
• Strings are interned only at compile-time, this means that they will not be interned if their value can't be computed at compile-time.

s1 = 'iambenwen'
s2 = 'iambenwen'
print(s1 is s2)
>true

>>> "strin"+"g" is "string"
True

- Python won’t intern all the string. 
>>> c = 'Y'*4097
>>> d = 'Y'*4097
>>> c is d
false

s1 = "wen".join(["b", "e", "n"])
s2 = "wen".join(["b", "e", "n"])
print(s1 is s2)
>false

>>> s1 = "strin"
>>> s2 = "string"
>>> s1+"g" is s2
False

- Don’t Rely on the Implicit String Interning
Because the rules of the implicit string interning could be different according to different compiler, interpreter. 
For example, up until the version of Python 3.7, the peephole optimization is used for string interning and all strings longer than 20 characters will not be interned. However, the algorithm was changed to the AST optimizer then, and the length is equal to 4096 rather than 20.

-- Explicit interning:

>>> import sys
>>> c = sys.intern('Y'*4097)
>>> d = sys.intern('Y'*4097)
>>> c is d
True

In practice, we should use the == operator to compare strings. If we need to speed up the comparison, intern the strings explicitly.

-- Disadvantages of String Interning
- Memory Cost.
- Time Cost: The call to intern() function is expensive as it has to manage the interned table.
- not friendly for Multi-threaded Environments .

-- The following comparison returns True because of shared objects. 
CPython loads the Latin-1 range of characters unicode decimals 0 to 255, inclusive, as shared objects every time Python is initialized. Any calls to values in this range are referred to those pre-existing objects.
s3 = 'levin'
s4 = 'elise'
print(s3[1] is s4[0])
>true


--- String Questions

-- If using u escape sequence follow by code point can display Unicode string. why need encoding?
s = "\u0041\u1234\u20ac\u8000\U0002070E"

print([ord(c) for c in s]) # get unicode’s integer value:
>[65, 4660, 8364, 32768, 132878]

print([chr(ord(i)) for i in s]) # get one-character string: 
>['A', 'ሴ', '€', '耀', '𠜎']

print(s.encode('utf-8').hex()) # get actually bytes of utf-8 encoding: 
>41e188b4e282ace88080f0a09c8e

print(s.encode('utf-16').hex()) # get utf-16
>fffe41003412ac20008041d80edf

print(s.encode('utf-32').hex()) # get utf-32
>fffe00004100000034120000ac200000008000000e070200

(The Unicode character U+FEFF is used as a byte-order mark (BOM), and is often written as the first character of a file in order to assist with auto detection of the file’s byte ordering. )

- ONE important concept is that, in Python, only refer to store or transmit strings when talking about encodings. 

- Using code point can display unicode string! utf-32 can be represented by codepoint totally. But this encoding have some shortcoming. like consuming too many space, byte ordering issue. 
- Python say it represent string with code point internally. it is possible and it is good to support string methods. The encoding can be ASCII, or USC2, or USC4. 
But user can store or transmit string by UTF-8

-- How to change str encoding.
In Python, str type is Unicode string. str’s encoding just for store or transmission. str.encode() support around 100 encoding type. 

-- How to know the encoding of one String? How do Python identify the encoding of one string?
Encoding is out-of-band. You cannot infer the encoding of bytes. You must be told, or you have to guess.

-- How to judge the string if we just get part of the string?
UTF-8 performing better at this issue.

-- How to indexing, slicing, getting length of a Unicode String?
Variable length encoding can make indexing slicing getting length fail. 
Python’s string type is using Unicode, but internal encoding is vary which depending on what characters the string contains. 

-- What is the relationship between string interning and encoding?
Not relative

--- List

- List is ordered and mutable, Allows duplicate members, similar to array in JavaScript.
- Lists are created using square brackets. Example:
list1 = ["abc", 34, True, 40, "male"]

- If you want to determine something inside of a List, that is big O(n) time complexity. It look over most of the elements of the list. But for Set or Dict, it is constant time complexity.
- If you pop the last element of the list, it is constant time operation. But remove the first element, then it is O(n) operation.
- If you need to have an order collection of elements, you want to pop at the end or beginning or middle of it. You should use the following two data structure:
from collections import deque, queue

--- list comprehension syntax

List comprehension is an elegant way to create lists based on existing lists.

--- Syntax:
newlist = [expression for item in iterable if condition == True]

- A list comprehension consists of brackets containing an expression followed by a for clause, then zero or more for or if clauses.

- The result will be a new list resulting from evaluating the expression in the context of the for and if clauses which follow it. For example, this listcomp combines the elements of two lists if they are not equal:

>>>[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

- Key point: the first for loop is outer loop. But you can use bracket to wrap expression and for loop together, and make second for loop outside:

matrix = [[1, 2], [3,4], [5,6], [7,8]]

# Flat 2D list to [1, 2, 3, 4, 5, 6, 7, 8]:
print([j for i in matrix for j in i])

# Transpose to [1, 3, 5, 7], [2, 4, 6, 8]]:
print([[row[i] for row in matrix] for i in range(2)])

--- Features
-- The condition is like a filter, and it is optional:
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
newlist = [x for x in fruits if "a" in x]
print(newlist) # ['apple', 'banana', 'mango']

-- The expression can also contain conditions, not like a filter, but as a way to manipulate the outcome:
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
newlist = [x if x != "banana" else "orange" for x in fruits]
print(newlist) # fruits = ["apple", "orange", "cherry", "kiwi", "mango"]

-- Support nested list comprehension

--- Common methods

--- Access List items
- s[i]     ith item of s, origin 0
- s[i:j]    slice of s from i to j
- s[i:j:k]  slice of s from i to j with step k

--- Modifying List
- append() method
- insert() method
- support operator: +, *, +=, *=

--- Remove List Items
- remove() method removes the specified item
- pop() method removes the specified index.
- The del keyword also removes the specified index
- clear() method empties the list.

--- reverse()

--- sort() 
- It will sort the list alphanumerically, ascending, by default.
- Sort the list descending:
lst.sort(reverse = True)

--- Copy a list
Thre are 3 ways to copy a list: 
    1. newlst = lst.copy()
    2. newlst = list(lst)
    3. newlst = [i for i in lst]

--- Join two list
1, list3 = list1 + list2
2, Another way to join two lists is by appending all the items from list2 into list1, one by one.
3, Use the extend() method to add list2 at the end of list1
list1.extend(list2)

--- Multi-Dimensional Arrays Operation

- When deal with Multi-Dimensional Arrays, it is good to use NumPy. 

-- What do X[:,1] means

- X is a numpy array, and it is Multi-Dimensional Array.
- Say Here X have n rows and n columns
- so by doing x=x[:,1] we get all the rows in x present at index 1.

-- for example:

x = array([ [0.69859393, 0.1042432 ],
         [0.55138493, 0.18639614],
         [0.27338772, 0.80351282]])

x[:,1] = array([0.1042432 , 0.18639614, 0.80351282])


--- Understanding Lists in Python

--- What is Array in Python

- An array is a set of elements which 
① have the same size, 
② located in memory one after another, without gaps.

- Since the “get value by address” memory operation takes constant time, selecting an array item by index also takes O(1).

-- Array vs. List



--- List is kind of array

- The list is based on the array. 

-- List = Array of Pointers
- The list instantly retrieves an item by index (O(1)), because it has an array inside. 
- And the array is so fast because all the elements are the same size.
- For example:
>>>sys.getsizeof(['1'])
64
>>>sys.getsizeof(['1','2'])
72
>>>sys.getsizeof(['a long long long string'])
64

-- List = Dynamic Array
- The list juggles arrays all the time so that we don’t have to do it . For example, if array is full, it will allocate new memory for all elements. 
- You can even check all detail in Python c code.

-- these operations are O(1):
- select an item by index lst[idx]
- count items len(lst)
- add an item to the end of the list .append(item)
- remove an item from the end of the list .pop()

-- operations are “slow”:
- Insert or delete an item by index. .insert(idx, item) and .pop(idx) take linear time O(n) because they shift all the elements after the target one.
- Search or delete an item by value. item in lst, .index(item) and .remove(item) take linear time O(n) because they iterate over all the elements.
- Select a slice of k elements. lst[from:to] takes O(k).

--- Conclusion
- Now you can understand why Memory of list is like that: it depond on how many members instead of how big of the members. 
- You can understand why some operations are quick, but some are slow.

--- Questions

--- Do Python allocate new memory for the following variable small_list?
big_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
small_list = big_list[:5]
Ask yourself if you modify small_list, do big_list modify as well? if not, then new memory.

--- Tuple 

--- What is Tuple

- A tuple is a sequence which is ordered and Immutable, Allows duplicate.
- Tuples are unchangeable, meaning that we cannot change, add or remove items after the tuple has been created. However, if a member of the tuple is list, list content can be changed. 
- But there is a workaround. You can convert the tuple into a list, change the list, and convert the list back into a tuple.
- Tuples are written with round brackets:
tuple1 = ("abc", 34, True, 40, "male")

-- Tuple Features

- Tuple is immutable, but it will instantiated without checking memory. It don’t like many immutable, e.g. int, float, check memory for same value instance. 
- Tuple have quirk syntax when it contain 0 or 1 items. 
t = ()  
t = (1,)

-- Access Tuple. 
- using []. 
- using keyword ‘in’.

-- Update Tuple
Convert the tuple into a list, update, and convert it back into a tuple.

-- Unpacking a Tuple
When create a tuple, normally assign values to it. This is called "packing" a tuple.
Tuple allowed to extract the values back into variables. This is called "unpacking". (Call destructuring assignment in JavaScript)

fruits = ("apple", "banana", "cherry")
(green, yellow, red) = fruits

Add an * to the variable name and the values will be assigned to the variable as a list:
fruits = ("apple", "banana", "cherry", "strawberry", "raspberry")
(green, yellow, *red) = fruits

-- You join Tuples by using ‘+’, multiply Tuples by using ‘*’

--- Range

- Range is built-in type in Python.
- The range type represents an immutable sequence of numbers and is commonly used for looping a specific number of times in for loops.

--- Range is not a generator

- Many people have some misunderstanding, but range is not a generator, it's not any kind of iterator. 
- If it were a generator, iterating it once would exhaust it, but:
>>> a = range(5)
>>> print(list(a))
[0, 1, 2, 3, 4]
>>> print(list(a))
[0, 1, 2, 3, 4]

--- Range is sequence

- Range actually is a sequence. it is a smart sequence object that produces numbers on demand.
- Range doesn't remember all of its values, it just remembers its start, stop, and step, and creates the values on demand on __getitem__.
- The range object implements the object.__contains__ hook, and calculates if your number is part of its range when you use keyword in. There is never a need to scan through all possible integers in the range.

- The difference between a range and a list is that a range is a lazy or dynamic sequence; 

- The advantage of the range type over a regular list or tuple is that a range object will always take the same (small) amount of memory, no matter the size of the range it represents (as it only stores the start, stop and step values, calculating individual items and subranges as needed).

--- Set

--- What is Set
A set object is an unordered collection of distinct hashable objects.
Like other collections, sets support x in set, len(set), and for x in set. 
Python have set and frozen set.

- Set support comprehension syntax as well, similar to list comprehension.

-- Common uses include 
- membership testing, 
- removing duplicates from a sequence, 
- Provide computing mathematical operations such as intersection, union, difference, and symmetric difference. 

--- set
These represent a mutable — the contents can be changed using methods like add() and remove(). 
Since it is mutable, it has no hash value and cannot be used as either a dictionary key or as an element of another set. 

- A set is a collection which is unordered, Not allow duplicates.
- We cannot change the items after the set has been created. but you can remove items and add new items.
- Sets are written with curly brackets:
set1 = {"abc", 34, True, 40, "male"}

--- frozenset
These represent an immutable set. 
As a frozenset is immutable and hashable, it can be used again as an element of another set, or as a dictionary key.

--- Instances of set and frozenset provide the following operations:
(You can tell what set and frozenset can do according this operations)

-- len(s)
Return the number of elements in set s (cardinality of s).

-- x in s
Test x for membership in s.

-- x not in s
Test x for non-membership in s.

-- isdisjoint(other)
Return True if the set has no elements in common with other. Sets are disjoint if and only if their intersection is the empty set.

-- issubset(other)
set <= other
Test whether every element in the set is in other.

-- set < other
Test whether the set is a proper subset of other, that is, set <= other and set != other.

-- issuperset(other)
set >= other
Test whether every element in other is in the set.

-- set > other
Test whether the set is a proper superset of other, that is, set >= other and set != other.

-- union(*others)
set | other | ...
Return a new set with elements from the set and all others.

-- intersection(*others)
set & other & ...
Return a new set with elements common to the set and all others.

-- difference(*others)
set - other - ...
Return a new set with elements in the set that are not in the others.

-- symmetric_difference(other)
set ^ other
Return a new set with elements in either the set or other but not both.

copy()
Return a shallow copy of the set.

--- operations available for set, but not frozenset

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

--- Questions

-- Can Set contains mutable type, like list? 
NO, elements in set, key in dictionary, have to be hashable. That means it can’t be list or other mutable type. Tuple can be elements of set if it haven’t mutable member.

--- Dictionary

--- What is Dictionary

Dictionaries are used to store data values in key:value pairs.
A dictionary is a collection which is ordered*, mutable and do not allow duplicates.

--- Features

- keys, which can be any immutable type; strings and numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple contains any mutable object either directly or indirectly, it cannot be used as a key. 

- As of Python version 3.7, dictionaries are ordered. In Python 3.6 and earlier, dictionaries are unordered.

- Use curl bracket:
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}

- The dict() constructor builds dictionaries directly from sequences of key-value pairs:
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'guido': 4127, 'jack': 4098}

- dict comprehensions can be used to create dictionaries from arbitrary key and value expressions:
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}

--- Operation Methods

--- Accessing Items

- Dictionary is in order. But have no idea to access it by order.

-- Access directly
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
}
print(thisdict["model"])

- using square bracket [] is equivalent to __getitem__()

- if visit key that doesn’t exist
It will raise KeyError.
But you can override the __missing__(self, key) method defines the behavior of a dictionary subclass if you access a non-existent key. 
More specifically, Python’s __getitem__() dictionary method internally calls the __missing__() method if the key doesn’t exist. The return value of __missing__() is the value to be returned when trying to access a non-existent key.


-- keys(), values(), items() 
- return view objects for corresponded things. 
- Dictionary view object provide a dynamic view on the dictionary’s entries, which means that when the dictionary changes, the view reflects these changes.
- Keys views are set-like!
- If all values are hashable, then the items view is also set-like.
- Values views are not treated as set-like.
- For set-like views, all of the operations defined for the abstract base class collections.abc.Set are available (for example, ==, <, or ^).

-- setdefault(key[, default])
Returns the value of the specified key. If the key does not exist: insert the key, with the specified value. 
- keyname is required
- value is Optional. If the key exist, value has no effect.
If the key does not exist, this value becomes the key's value. Default value None. 
print(thisdict.setdefault("model","Focus"))

-- get(key[, default])
Return the value for key if key is in the dictionary, else default. If default is not given, it defaults to None.
- get() is different function from __getitem__(). They have no connection.

--- Remove items

-- pop()
The pop() method removes the item with the specified key name.
-- popitem()
The popitem() method removes the last inserted item (in versions before 3.7, a random item is removed instead):
-- del keyword
Can delete items or the dictionary
-- clear()
Clear the dictionary

--- Add items, Change items

-- add item directly, change directly
(list can’t add item directly, but dict can)
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
}
thisdict["color"] = "red"
thisdict["model"] = "Focus"

- using square bracket [] is equivalent to __setitem__()

-- update()
The update() method inserts the specified items to the dictionary.
- update() can overwrite the exist key and it’s value. 
- can concatenate dictionarys. 
- can concatenate key value pair iterable object.
dictionary.update(iterable)
 • iterable can be A dictionary or an iterable object with key value pairs, that will be inserted to the dictionary. 

-- dict NOT support operator +
But Counter can

-- setdefault()
see above

--- Create Dictionary

-- dict.fromkeys(keys, value)
The fromkeys() method returns a dictionary with the specified keys and the specified value.

- syntac
dict.fromkeys(keys, value)
keys: Required. An iterable specifying the keys of the new dictionary
value: Optional. The value for all keys. Default value is None

- an example of how to use dict as an ordered set to filter out duplicate items while preserving order, thereby emulating an ordered set:
>>> keywords = ['foo', 'bar', 'bar', 'foo', 'baz', 'foo']
>>> list(dict.fromkeys(keywords))
['foo', 'bar', 'baz']

-- zip()
zip() returns an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument iterables.
keys = ['red', 'green', 'blue']
values = ['#FF0000','#008000', '#0000FF']
color_dictionary = dict(zip(keys, values))

--- Copy Dictionary
-- copy()
Dictionary method copy() can create a new dict according to the current dict.

-- dict()
Another way to make a copy is to use the built-in function dict().

--- Loop over dict
for x in thisdict:
  print(x)

x is the key! no value. 

--- Question

--- How to get the first item of a dict?


--- Collections

--- Time Complexity of Collections

--- list

- Internally, a list is represented as an array; 
- The largest costs come from growing beyond the current allocation size (because everything must move), or from inserting or deleting somewhere near the beginning (because everything after that must move). 
- If you need to add/remove at both ends, consider using a collections.deque instead.



--- dict 

- There is a fast-path for dicts that (in practice) only deal with str keys; 

- Python have a typical space-time tradeoff in dictionaries and lists. It means we can decrease the time necessary for our algorithm but we need to use more space in memory for dictionaries.

- time complexity x in s operation for dict is O(1); But for list if O(n).



--- set

See dict -- the implementation is intentionally very similar.



--- collections.deque

A deque (double-ended queue) is represented internally as a doubly linked list. (Well, a list of arrays rather than objects, for greater efficiency.) Both ends are accessible, but even looking at the middle is slow, and adding to or removing from the middle is slower still.



--- collections.defaultdict

(you can write a subclass of dictionary which is same with defaultdict easily)

--- What is defaultdict
- defaultdict(default_factory=None) Return a new dictionary-like object. 
- The first argument provides the initial value for the default_factory attribute; it muse be callable or None.
- You can following function to name a constant value for the default_factory:
def constant_factory(value):
    return lambda: value

- defaultdict is a subclass of the built-in dict class. It overrides one method (__missing__()) and adds one variable (default_factory). The remaining functionality is the same as for the dict class.
- __missing__() is called by the __getitem__() method of the dict class when the requested key is not found;

--- Why need defaultdict 
- you can define the default type for the dictionary value. Then when you can’t find the corresponded value, you will get default value and it’s type and use corresponded method.

--- Following example can to understand defaultdict!

-- Example1: 
Using list as the default_factory, it is easy to group a sequence of key-value pairs into a dictionary of lists:

s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = defaultdict(list)
for k, v in s:
    d[k].append(v)

# d will be {'yellow': [1, 3], 'blue': [2, 4], 'red': [1]})

-- Example2: 
Setting the default_factory to int makes the defaultdict useful for counting (like a bag or multiset in other languages):

s = 'mississippi'
d = defaultdict(int)
for k in s:
    d[k] += 1

sorted(d.items())
[('i', 4), ('m', 1), ('p', 2), ('s', 4)]

-- Example3:
Setting the default_factory to set makes the defaultdict useful for building a dictionary of sets:

s = [('red', 1), ('blue', 2), ('red', 3), ('blue', 4), ('red', 1), ('blue', 4)]
d = defaultdict(set)
for k, v in s:
    d[k].add(v)

sorted(d.items())
[('blue', {2, 4}), ('red', {1, 3})]

--- collections.Counter

--- What is Counter
- It is a collection where elements are stored as dictionary keys and their counts are stored as dictionary values. 
- Counts are allowed to be any integer value including zero or negative counts. 
- A Counter is a dict subclass for counting hashable objects. 

-- Syntax:
 collections.Counter([iterable-or-mapping])

-- Features
- The usual dictionary methods are available for Counter objects except for two which work differently for counters. They are udpate(), fromkeys()
- Counter support additional methods: elements(), most_common(), total(), subtract()
- Counter support operator + , - , &, |

--- Why Counter
Following examples is common way to use counter. They should give you some ideas:

-- Get common items
>>>Counter('abracadabra').most_common(3)
[('a', 5), ('b', 2), ('r', 2)]

-- list elements
>>>c = Counter(a=4, b=2, c=0, d=-2)
>>>sorted(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']

-- counter + counter
>>>c = Counter(a=3, b=1)
>>>d = Counter(a=1, b=2)
>>>c + d      # add two counters together:  c[x] + d[x]
Counter({'a': 4, 'b': 3})

-- counter[key] +=
>>>c = Counter(a=3, b=1)
>>>d = Counter(a=1, b=2)
>>>c['a'] += d ['a'] 
Counter({'a': 4, 'b': 1})


--- Context Manager Types

--- What is Context Manager

(https://peps.python.org/pep-0343/)

- A context manager is an object that defines the runtime context to be established when executing a with statement.

- The context manager handles the entry into, and the exit from, the desired runtime context for the execution of the block of code. 

- Context managers are normally invoked using the with statement, but can also be used by directly invoking their methods.

--- The with statement
The with statement is used to wrap the execution of a block with methods defined by a context manager. This allows common try…except…finally usage patterns to be encapsulated for convenient reuse.
