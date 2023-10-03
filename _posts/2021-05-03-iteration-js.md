---
layout: post
title: "The Iteration in JS"
date: 2021-05-03
categories: JavaScript
tags: Iterable Iterator 
---

	
## Iterable

Iterable object is the object which conform with iterable protocol. 

Then What is Iterable protocol?
Simply say, implementing the **@@iterator** method.
That means object (or one of the objects up its prototype chain) must have a property with `[Symbol.iterator]` key. 
		
- Iterable protocol allow Object to define or customize their iterative operation. Such as what values are looped over in a `for...of` structure.

- Some build-in type is build-in iterables. Such as Array, Map, String. Because they implements `@@iterator` method. But object is not iterator.	

- Iterables which can iterate only once (such as Generators) customarily return this from their `@@iterator` method, whereas iterables which can be iterated many times must return a new iterator on each invocation of `@@iterator`.

- `for...of` , spread operator and `yield*` Syntaxes expecting iterables.

## Iterator
 
Any objects conform with iterator protocol and then it is iterator object.
It is rarely desirable to implement iterator protocol without also performing iterable.

Iterator protocol:  
Define a standard way to produce a sequence of values.
An object can be call iterator when it implements the **next()** according to some semantics (what Symbol.iterator method return is iterator object):

Basic semantics like below code:
```javascript
function range (from = 0, to = Infinity) {
  return {
    current: from,
    last: to,
    next: function (){
      if (this.current < this.last){
        return {done: false, value: this.current++}
      }else {
        return {done: true}
      }
    }
  }
}
let myRange = range (4, 10);
let iter = myRange.next();
while (!iter.done){
  console.log(`iterator value: ${iter.value}`)
  iter = myRange.next();
}
```

I believe Iterator use closure to maintain the variables so that the next() can re-enter and get value. 

You can create your own Iterator object by two ways:
- implements the next() fucntion according to some semantics. 
- use generator. 

## Generator 

### Firstly, Why we need Generator

While custom iterators are a useful tool, their creation requires careful programming due to the need to explicitly maintain their internal state. 
Generator functions provide a powerful alternative: they allow you to define an iterative algorithm by writing a single function whose execution is not continuous. 

Generator Benifit:
- Iterators and Generators bring the concept of iteration directly into the core language.
- Generators in JavaScript -- especially when combined with Promises -- are a very powerful tool for asynchronous programming as they mitigate -- if not entirely eliminate -- the problems with callbacks, such as Callback Hell and Inversion of Control. However, an even simpler solution to these problems can be achieved with async functions.

### What is Generator

Generators are functions that can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances. In JavaScript, Normal function follows something called run-to-completion model. But Generator is **special**. 

- Calling a generator function does not execute its body immediately; an iterator object (can call generator object) for the function is returned instead. This Generator object conforms to both the iterable protocol and the iterator protocol.

- When the iterator's next() method is called, the generator function's body is executed until the first yield expression, which specifies the value to be returned from the iterator or, with yield*, delegates to another generator function.

- Because generator is iterable, it implement @@iterator method. Generator iterate only once, and it return this from their @@iterator method.

- In short, a generator appears to be a function but it behaves like an iterator.
{% include figure.html path="assets/img/Generator-js.png" class="img-fluid rounded z-depth-1" %}

This is a simple generator:
```javascript
function* idMaker() {
  var index = 0;
  while (true)
    yield index++;
}
var gen = idMaker();
console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen[Symbol.iterator]() === gen) // true 
```

Make your own iterables suing generator:
```javascript
const myIterable = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
}
for (let value of myIterable) {
    console.log(value);
}
```

You can use Promises, generators to mimic the async/await functionality. 

### What is Generator Function

Generator function is function object, declared by function*, or can be declared by GeneratorFunction constructor.
		
The Difference between Yield and Return in Generator function:
1. Yield and Return both return Generator object.
2. Yield keep Generator 's done as false and keep function looking for next yield. 
3. Return statement in a generator, when executed, will make the generator finish 
(i.e. the done property of the object returned by it will be set to true). 

Example of Using next()
```javascript
function* generator(i) {
  yield i;
  yield i + 10;
}
const gen = generator(10);
console.log(gen.next().value); // expected output: 10
console.log(gen.next().value); // expected output: 20
```

Calling the next() with an argument.
will resume the generator function execution, replacing the yield expression where an execution was paused with the argument from next().

```javascript
function* generator(i) {
  yield i;
  return i + (yield i);
}
const gen = generator(10);

console.log(gen.next()); // Object { value: 10, done: false }
console.log(gen.next()); // Object { value: 10, done: false }
console.log(gen.next(10));// Object { value: 20, done: true } 
```

The 10 in next(10) will replace the second (yield). So it is return i + 10. That is why we got { value: 20, done: true } .

-- Using yield

	
-- using yield*, delegates to another generator function. 		
		
### Generator use some technique like closure.
clouse:
```javascript
const numberIncrementer = startValue => () => startValue++
const getNextNumber = numberIncrementer(0)
console.log(getNextNumber()) // 0
console.log(getNextNumber()) // 1
console.log(getNextNumber()) // 2
```
generator:
```javascript
const numberIncrementer = function*(startValue) {
    while(true) {
        yield startValue++
    }
}
const numberFactory = numberIncrementer(0)
const getNextNumber = () => numberFactory.next().value
console.log(getNextNumber())// 0
console.log(getNextNumber())// 1
console.log(getNextNumber())// 2
```

### Generator Object

The Generator cannot be instantiated directly. Instead, a Generator instance can be returned from a generator function (constructor function).
Generator Object inherits properties and methods from: Iterator.
Generator is a Iterable, and also a Iterator.

Generator Instance methods
- Generator.prototype.next()
Returns a value yielded by the yield expression.
- Generator.prototype.return()
Returns the given value and finishes the generator.
- Generator.prototype.throw()
Throws an error to a generator (also finishes the generator, unless caught from within that generator).

### Using Generator and Promise to mimic async/await 

Code using promises and callbacks such as:
```javascript
function fetchJson(url) {
    return fetch(url)
    .then(request => request.text())
    .then(text => {
        return JSON.parse(text);
    })
    .catch(error => {
        console.log(`ERROR: ${error.stack}`);
    });
}
```
can be written as (with the help of libraries such as co.js):

```javascript
const fetchJson = co.wrap(function * (url) {
    try {
        let request = yield fetch(url);
        let text = yield request.text();
        return JSON.parse(text);
    }
    catch (error) {
        console.log(`ERROR: ${error.stack}`);
    }
});
```
Some readers may have noticed that it parallels the use of async/await. That’s not a co-incidence. async/await can follows a similar strategy and replaces the yield with await in cases where promises are involved.


## for loop 

### for...of
When you use for...of, we need to add a method to the object named Symbol.iterator.

1. When for...of starts, it calls Symbol.iterator method once (or errors if not found). The method must return an iterator - an object with the method next.
2. Onward, for...of works only with that returned object.
3. When for...of wants the next value, it calls next() on that object.
4. The result of next() must have the form {done: Boolean, value: any}, where done=true means that the loop is finished, otherwise value is the next value.

### Difference between for...in and for...of

- for...in statement iterates over all non-symbol and enumerable properties of an object. it is used for OBJECT.
- for...of statement create a loop iterating over iterable object. it is for ITERATOR OBJECT. include all build-in iterable type. 
- when iterate over an array, prefer use for...of .
- for...in, the most practically used is for debug.
- object.hasOwnProperty() means the property is not inherited.

```javascript
function myForOfForIn() {
  Object.prototype.objCustom = function() {};
  Array.prototype.arrCustom = function() {};
  const iterable = [3, 5, 7];
  iterable.foo = 'hello';
  for (const i in iterable) {
    console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
  }
  for (const i in iterable) {
    if (iterable.hasOwnProperty(i)) {
      console.log(i); // logs 0, 1, 2, "foo"
    }
  }
  for (const i of iterable) {
    console.log(i); // logs 3, 5, 7
  }
}
```
