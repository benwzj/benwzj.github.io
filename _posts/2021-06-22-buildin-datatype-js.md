---
layout: post
title: Build-in Data type in JS
date: 2021-06-22
category: JavaScript
tags: JavaScript String
toc: 
  - name: String
  - name: Number
  - name: BigInt
  - name: Symbol
  - name: Array
  - name: Set
  - name: Map
  - name: WeakMap
---

JavaScript has two primitive values used to signal absent or uninitialized value: `null` and `undefined`.

## String

- Strings can be created as primitives from string literals, or as objects, using the String() constructor. 
const string1 = "A string primitive";
const string2 = new String("A String object");
- Can use ==, >, < to Compare strings.
- JavaScript distinguishes between String objects and primitive string values. 
- A String object can always be converted to its primitive counterpart with the valueOf() method.

> ##### JavaScript strings are all UTF-16 sequences. 
>
>ECMAScript standard says:
When a String contains actual textual data, each element is considered to be a single UTF-16 code unit.
{: .block-warning}

### Using Unicode in a string
A unicode sequence can be added inside any string using the format \uXXXX:
`const s1 = '\u00E9' //é`
A sequence can be created by combining two unicode sequences:
```js
const s2 = '\u0065\u0301' //é
const s3 = 'e\u0301'
s2 === s3 // true
s2.length = 2
s1 !== s3 // true
But can 
s1.normalize() === s3.normalize() // true
```

### Emojis
Emojis are Unicode characters.
The 🐶 symbol, which is `U+1F436`, is traditionally encoded as `\uD83D\uDC36` (called surrogate pair. There is a formula to calculate this), Or be represented as `\u{1F436}`

### The static `String.raw()` method

It is a tag function of template literals. It's used to get the raw string form of template strings, that is, substitutions (e.g. `${foo}`) are processed, but escapes (e.g. `\n`) are not.
**So, raw string is relative to template string, and do not substitute escape**

A raw string literal is a string in which the escape characters (like \n \t or \" ) of C++ are not processed. That means raw is easier to include escape characters. They’re useful for, say, encoding text like HTML. 
```js
const filePath = String.raw`C:\Development\profile\aboutme.html`;
console.log(filePath);
```

### split()
It turns a String into an array of strings, by separating the string at each instance of a specified separator string.
```js
const chars = str.split(''); // split every char into an array;
const words = str.split(' '); // split every word into an array;
```
### Trim() 
It removes whitespace from both ends of a string. 
Whitespace in this context is all the whitespace characters (space, tab, no-break space, etc.)
and all the line terminator characters (LF, CR, etc.).

### charAt() 
return a new String and carry one letter;
```js
'cat'.charAt(1) // returns "a"
'cat'[1] // returns "a" in ECMAScript 5, treat the string as an array-like object, where individual characters correspond to a numerical index.
```

> In JS, there are no char operation. all in String object. 

- capitalize a letter: 
`String.charAt(i).toUpperCase();`
- difference bewteen charAt(i) and string[i]:
`str.charAt(0)` will return String `('')` for `str = ''` instead of `undefined` in case of `''[0]`;

### String.prototype.codePointAt() vs String.prototype.charCodeAt()

- The charCodeAt() method returns an integer between 0 and 65535 representing the UTF-16 code unit at the given index.
If the Unicode code point cannot be represented in a single UTF-16 code unit (because its value is greater than 0xFFFF) then the code unit returned will be the first part of a surrogate pair for the code point. If you want the entire code point value, use codePointAt().

- The codePointAt() method returns a non-negative integer that is the Unicode code point value at the given position. 
```js
const icons = '對';
console.log(icons.codePointAt(0));
// > 23565
```
Return value: 
If the element at pos is a UTF-16 high surrogate, returns the code point of the surrogate pair.
If the element at pos is a UTF-16 low surrogate, returns only the low surrogate code point.
```js
'\ud83d\ude0d'.codePointAt(0).toString(16)  //return code point of surrogate pair: 1f60d
'\ud83d\ude0d'.codePointAt(1).toString(16)  //return just low surrogate code: de0d
```

### About empty string ''
```js
!'' //->true
!!'' // -> false
!undefined // true

//false, 0, undefined, null is special value. 
// following always return the last one 

''||false //-> false
false||'' //->''
undefined || 0 //-> 0
0||undefined //-> undefined
'' || undefined //->undefined
undefined || '' //->''
null || '' //->''
```

### Template Strings

Template Strings use back-ticks (``) rather than the quotes ("") to define a string:
```js
let text = `Hello World!`;
```
Template Strings allow both single and double quotes inside a string.
Template Strings allow multiline strings.
Template Strings allow variables in strings.
```js
let firstName = "John";
let lastName = "Doe";
let text = `Welcome ${firstName}, ${lastName}!`;
```

## Number

Number is a primitive wrapper object used to represent and manipulate numbers like 37 or -9.25.
The JavaScript Number type is a double-precision 64-bit binary format IEEE 754 value, like double in Java or C#.

### Features
- A number literal like 37 in JavaScript code is a floating-point value, not an integer.
There is no separate integer type in common everyday use. 
- Number may also be expressed in literal forms like 0b101, 0o13, 0x0A.
- When use Number(value) converts a string or other value to the Number type. If the value can't be converted, it returns NaN.
- Number.MAX_SAFE_INTEGER
The maximum safe integer in JavaScript (2^53 - 1).
- The global property Infinity is a numeric value representing infinity.

### Number.NaN
Special "Not a Number" value.

- The Number.NaN is Equivalent of NaN. But Number.isNaN() is different from isNaN().
- NaN is a property of the global object. In other words, it is a variable in global scope.
- console.log(typeof(NaN)); // -->number
- NaN compares unequal (via ==, !=, ===, and !==) to any other value — including to another NaN value. 
- Use Number.isNaN() or isNaN() to most clearly determine whether a value is NaN. 

#### There are five different types of operations that return NaN:

- Number cannot be parsed (e.g. parseInt("blabla") or Number(undefined))
- Math operation where the result is not a real number (e.g. Math.sqrt(-1))
- Operand of an argument is NaN (e.g. 7 ** NaN)
- Indeterminate form (e.g. 0 * Infinity, or undefined + undefined)
- Any operation that involves a string and is not an addition operation (e.g. "foo" / 3)

#### the difference between isNaN() and Number.isNaN(): 
the former will return true if the value is currently NaN, or if it is going to be NaN after it is coerced to a number, while the latter will return true only if the value is currently NaN:
```js
isNaN('hello world');        // true
Number.isNaN('hello world'); // false
```

## BigInt
A BigInt value, also sometimes just called a BigInt, is a bigint primitive, created by appending n to the end of an integer literal, 

## Symbol

Symbol is a built-in object whose constructor returns a symbol primitive — also called a Symbol value or just a Symbol — that's guaranteed to be unique. 

### Main purpose

- Symbols are often used to add unique property keys to an object that won't collide with keys any other code might add to the object, and which are hidden from any mechanisms other code will typically use to access the object.

- Before symbols were introduced, object keys were always strings or indexing, so they were easy to overwrite. Also, it was common to have name conflicts when using multiple libraries.
```js
let user = {};
user.userName = 'User name';
user.userID = 123;
let symID = Symbol('symID');
user[symID] = 321;
```
### Syntax
To create a new primitive Symbol, you write Symbol() with an optional string as its description:
`let sym = Symbol('foo')`

### It does not support the syntax "new Symbol()". 
It will throw TypeError:  
`let sym = new Symbol()  // TypeError`

This prevents authors from creating an explicit Symbol wrapper object instead of a new Symbol value.
If you want to create a Symbol wrapper object, you can use the Object() function:
```js
let sym = Symbol('foo')
let symObj = Object(sym)
```
### Synbol Features

- symbol property in object are not enumerable. 
Means they will not come out in `Object.key()` or `for...in`. 
But can use `Object.getOwnPropertySymbols()`.
They will not show up in string which created by `JSON.stringify()`.

- Shared Symbols in the global Symbol registry
Symbols are designed to be unique, but you still can be share it through global symbol registry.
Using the `Symbol()` function syntax will not create a global Symbol that is available in your whole codebase.
To create Symbols available across files and even across realms (each of which has its own global scope), use the methods `Symbol.for()` and `Symbol.keyFor()` to set and retrieve Symbols from the global Symbol registry.
```js
console.log(Symbol.for('bar') === Symbol.for('bar')); // bar is key
// expected output: true

console.log(Symbol('bar') === Symbol('bar')); // bar is description
// expected output: false

const symbol1 = Symbol.for('foo');
const symbol2 = Symbol('bar');

console.log(symbol1.toString());
// expected output: "Symbol(foo)"
console.log(symbol2.toString());
// expected output: "Symbol(bar)"
```

### Symbol.iterator
`Symbol.iterator` is Symbol static property. `Symbol.iterator` is special built-in symbol which just iterable. It is unique and always same.
The well-known` Symbol.iterator` symbol specifies the default iterator for an object. Used by `for...of`.
```js
const iterable1 = {};
iterable1[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};
console.log([...iterable1]);
// expected output: Array [1, 2, 3]
```

## Array

### Overview:
- Array is an special object, but it store data in the same way with object. For example, when you visit data through `arr[2]`, The `2` in `arr[2]` is coerced into a **string** by the JavaScript engine through an implicit toString conversion. As a result, '2' and '02' would refer to two different slot.
- Support Group concept by `Array.prototype.groupBy()`
- Create an array from array-like object or iterable object through static method Array.from().
- Can Check if an array contains a certain item: includes(), and indexOf().

### Length can be weird

- Set menber by bracket notation
```js
const fruits = [];
fruits.push('banana');
fruits[3]= 'apple';
console.log(fruits); //Array ["banana", undefined, undefined, "apple"]
console.log(Object.keys(fruits).length) // 2
console.log(fruits.length); // 4
```
- Set length directly
```js
const fruits = [];
fruits.push('banana');
fruits.length = 3;
console.log(fruits);  // Array ["banana", undefined, undefined]
console.log(Object.keys(fruits).length) // 1

const numbers = [1, 2, 3, 4, 5];
numbers.length = 3;
console.log(numbers); // [1, 2, 3]
```
- delete a member with keyword delete.
```js
const fruits = [];
fruits.push('banana', 'apple', 'grape');
delete fruits[1];
console.log(fruits); //Array ["banana", undefined, "grape"]
console.log(Object.keys(fruits).length) // 2
console.log(fruits.length); // 3
```

### Basic Methods

#### slice() vs. splice()

- The `slice()` method returns a shallow copy of a portion of an array into a new array object selected from start to end (end not included) where start and end represent the index of items in that array. The original array will not be modified.
```js
let fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
let citrus = fruits.slice(1, 3)

// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```
- The `splice()` method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.
```js
let myFish = ['angel', 'clown', 'drum', 'sturgeon']
let removed = myFish.splice(2, 1, 'trumpet')

// myFish is ["angel", "clown", "trumpet", "sturgeon"]
// removed is ["drum"]
```
#### forEach()
THIS IS WEIRD METHOD.

forEach() calls a provided callbackFn function once for each element in an array in ascending index order. 
- syntax: forEach((element) => { /* ... */ })
- These callbackFns will keep running until for the last element. There is no way to stop or break a forEach() loop other than by throwing an exception. 
- forEach expects a synchronous function. forEach does not wait for promises, So don’t use.
- If modify the array during iteration, something weird will happen.

#### map() 
Create a new array with the resule which was provide by the function on every element of the calling array.
Don't use map() if you are not use the return array or the callback return nothing.

A, array.map (itme.index);
index is an optional argument. sometime can be used for key at React, although that is not the good choice.

#### flat() 
Creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

#### flatMap() 
First maps each element using a mapping function, then flattens the result into a new array. 
It is identical to a map() followed by a flat() of depth 1. 
```js
const recipients = 'ben.wen@gmail.com, benn@Gee.com ;xiao@gmail.com ,tone@gmail.com; tom@gmeil.com;heoo@hotmail.com,hei@gmi.com';
const emailArr = recipients.split(';').flatMap(x=>x.split(','))
console.log (emailArr);
```

#### split
`recipients.split(',').map(email=>{return {email}}) ;`
EQUAL TO: 
`recipients.split(',').map(email=> ({email})) ;`

For The code : recipients.split(',').map(email=> {email}) ; 
JavaScript syntax don't know what do {} mean: they are for object or function. 
So it need () around it for object.

#### concat()
use to merge two or more arrays into a NEW arr; concat() is not mutate original array.
Now, we all prefer to use '...'spread operator to do the job. 

#### reverse(); 
reverse an array in place. the first element become the last one. 
The reverse method transposes the elements of the calling array object in place, 
*mutating* the array, and returning a reference to the array.
it mutate the original array.

#### sort ();
The sort() method sorts the elements of an array in place and returns the sorted array. 
A, if no compareFunction, then the array elements are converted to strings, and sorted according to each character's Unicode code point value.
B, if compareFunction, we have a pattern:
```js
function compare(a, b) {
if (a is less than b by some ordering criterion) {
return -1;
}
if (a is greater than b by the ordering criterion) {
return 1;
}
// a must be equal to b
return 0;
}
```
#### join ()
creates and returns a new string by concatenating all of the elements in an array (or an array-like object), 
separated by commas or a specified separator string. If the array has only one item, then that item will be returned without using the separator.

#### reduce ()
Reduce() provide a very flexible way to handle array operations. it can replace of filter(), map(). Or remove the duplicate items. 
it will run over all the array elements and return a value for the next round everytime .

A, reducer() 's callback function carry 4 arguments, but usually use accumulator and currentValue: 
a, acc, first time, it is initialValue (if initialValue have been supplied) or first value; Then , it is callback's return values;
b, cur, first time, it is first value (if initialValue have been supplied) or second value; then run over all elements in the array.
B, reducer() second arugment is initialValue, usually it is been used. very useful.

C, important: **return acc in callback function;**

#### push() 
adds one or more elements to the end of an array . And return new length . 
Be careful at following code, It will push newnode after 'node' instead of the end of actionArr. 
```js
for ( let node of actionArr ){
  actionArr.push (...node.childrenArr);
}
```

#### group()

Syntax:
`group(callbackFn)`

Example:
```js
const inventory = [
  { name: 'asparagus', type: 'vegetables', quantity: 5 },
  { name: 'bananas',  type: 'fruit', quantity: 0 },
  { name: 'goat', type: 'meat', quantity: 23 },
  { name: 'cherries', type: 'fruit', quantity: 5 },
  { name: 'fish', type: 'meat', quantity: 22 }
];

let result = inventory.group( ({ type }) => type );

/* Result is:
{
  vegetables: [
    { name: 'asparagus', type: 'vegetables', quantity: 5 },
  ],
  fruit: [
    { name: "bananas", type: "fruit", quantity: 0 },
    { name: "cherries", type: "fruit", quantity: 5 }
  ],
  meat: [
    { name: "goat", type: "meat", quantity: 23 },
    { name: "fish", type: "meat", quantity: 22 }
  ]
}
*/
```
### Questions
- What is the Group concept in Array?

## Set

(Python has set built-in type, similar concept)
The Set object lets you store unique values of any type. 
Each value can only occur once in a Set.

- You can iterate through the elements of a set in insertion order. 
- One functionality of Set is Used to remove duplicate elements from the array. 
- In Python, it can use {} literal to create set. But it has no literal to create set in JavaScript.

- Performance
The Set has method checks if a value is in a Set object, using an approach that is, on average, faster than the Array.prototype.includes method when an Array object has a length equal to a Set object's size.

### Essential Method Property

Method:
- new Set()	 Creates a new Set
- add()	     Adds a new element to the Set
- delete()	 Removes an element from a Set
- has()	     Returns true if a value exists in the Set
- forEach()	 Invokes a callback for each element in the Set
- values()	 Returns an iterator with all the values in a Set

Property:
- size	Returns the number of elements in a Set

## Map

The Map object holds key-value pairs and remembers the original insertion order of the keys. Any value (both objects and primitive values) may be used as either a key or a value.

### Features

- A Map object iterates its elements in insertion order — a for..of loop returns an array of [key, value] for each iteration.
- Key equality is based on the sameValueZero algorithm. 
- If there are some keys are same, then just the last one will be keep in Map.
- key and value can't be swap. you can find value from key. but can't be reverse.
- you can use for..of , forEach(), to iterate through Map.
- You can say Map is a special array: two member array, first one for key, second one for value.
- You can say Array is just a special Map which key is 0, 1, 2 ...
- Map and Set work are designed in similar way, and they work in similar way. 
- Although Map's key can be any types, but usually just use primitive types. 
because when you look up the key, it use samevaulezero comparasion. Object types will just be compare the reference. it make no sense. 

### typical Map looks like below: 
```js
let myMap = new Map([
 [1, 'one'],
 [2, 'two'],
 [3, 'three'],
])
console.log(myMap) will display: 
{1 => 'one', 
2 => 'two',
3 => 'three'}
```

### Map vs. Array

I believe Map is built on top of Object with way as Array on top of Object.
```js
let kvArray = [['key1', 'value1'], ['key2', 'value2']] 
```

Use Map constructor to transform a 2D key-value Array into a map
```js
let myMap = new Map(kvArray)
myMap.get('key1') // returns "value1"
```

Use `Array.from()` to transform a map into a 2D key-value Array
```js
console.log(Array.from(myMap)) // exactly the same Array as kvArray
```
A succinct way to do the same, using the spread syntax
`console.log([...myMap])`

Use keys() or values() iterators, convert them to an array
```js
console.log(Array.from(myMap.keys())) // ["key1", "key2"]
```

### Map vs. Object

Object is similar to Map—both let you set keys to values, retrieve those values, delete keys, and detect whether something is stored at a key. For this reason, Object has been used as Map historically.

#### Key
- A Map's keys can be any value (including functions, objects, or any primitive).
- The keys of an Object must be either a String or a Symbol.

- A Map does not contain any keys by default. It only contains what is explicitly put into it.
- An Object has a prototype, so it contains default keys that could collide with your own keys if you're not careful.

#### Ordered Keys
- The keys in Map are ordered. Thus, when iterating over it, a Map object returns keys in order of insertion.
The keys of an Object are not ordered.

#### Size
- The number of items in a Map is easily retrieved from its size property.
- The number of items in an Object must be determined manually.

#### Iterable
- A Map is an iterable, so it can be directly iterated.
- Object does not implement an iteration protocol. But An object can implement the iteration protocol using Object.keys or Object.entries. On the other hand The for...in statement allows you to iterate over the enumerable properties of an object.

#### Perform
- Map Performs better in scenarios involving frequent additions and removals of key-value pairs.
- Object do not optimized for frequent additions and removals of key-value pairs.

#### Notation []
- Object can use property access notation []. But please don’t use [] to access Map's key and value. It will cause trouble. 

### Basic useage of Map:
```js
let myMap = new Map()
myMap.set (0, 'zero')
myMap.set (1, 'one')
```

#### Cloning Maps
Just like Arrays, Maps can be cloned:
```js
let original = new Map([[1, 'one']]);
let clone = new Map(original);
```
#### Merging Maps:
```js
let first = new Map([
[1, 'one'],
[2, 'two'],
[3, 'three'],
])
let second = new Map([
[1, 'uno'],
[2, 'dos']
])
let merged = new Map([...first, ...second])
```

Setting Object properties works for Map objects as well, and can cause considerable confusion.
So, always use Map methods to handle Map
use Map methods: `mymap.set('bla', 'blaa');`
Dont use: `mymap['bla'] = 'blaa'; // this is for Object`

### Main methods and proerty

- Method	
new Map()	 Creates a new Map
set()	      Sets the value for a key in a Map
get()	      Gets the value for a key in a Map
delete()	   Removes a Map element specified by the key
has()	      Returns true if a key exists in a Map
forEach()	  Calls a function for each key/value pair in a Map
entries()	  Returns an iterator with the [key, value] pairs in a Map

- Property
size	Returns the number of elements in a Map

## WeakMap

A WeakMap is a collection of key/value pairs whose keys must be objects, with values of any arbitrary JavaScript type, and which does not create strong references to its keys.

- An object's presence as a key in a WeakMap does not prevent the object from being garbage collected.

- WeakMap allows associating data to objects in a way that doesn't prevent the key objects from being collected, even if the values reference the keys. However, a WeakMap doesn't allow observing the liveness of its keys, which is why it doesn't allow enumeration;

### Why WeakMap

#### Map implementation would cause issue in garbage collection. 

A map API could be implemented in JavaScript with two arrays (one for keys, one for values) shared by the four API methods. Setting elements on this map would involve pushing a key and value onto the end of each of those arrays simultaneously. 
As a result, the indices of the key and value would correspond to both arrays. Getting values from the map would involve iterating through all keys to find a match, then using the index of this match to retrieve the corresponding value from the array of values.

Such an implementation would have two main inconveniences:

- The first one is an O(n) set and search (n being the number of keys in the map) since both operations must iterate through the list of keys to find a matching value.

- The second inconvenience is a memory leak because the arrays ensure that references to each key and each value are maintained indefinitely. These references prevent the keys from being garbage collected, even if there are no other references to the object. This would also prevent the corresponding values from being garbage collected.

#### WeakMap is designed to be easier to be collected.

By contrast, in a WeakMap, a key object refers strongly to its contents as long as the key is not garbage collected, but weakly from then on. As such, a WeakMap:

- does not prevent garbage collection, which eventually removes references to the key object
- allows garbage collection of any values if their key objects are not referenced from somewhere other than a WeakMap

A WeakMap can be a particularly useful construct when mapping keys to information about the key that is valuable only if the key has not been garbage collected.

But because a WeakMap doesn't allow observing the liveness of its keys, its keys are not enumerable. There is no method to obtain a list of the keys. If there were, the list would depend on the state of garbage collection, introducing non-determinism. If you want to have a list of keys, you should use a Map.
