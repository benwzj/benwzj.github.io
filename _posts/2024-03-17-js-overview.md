---
layout: post
title: JavaScript Overview
date: 2024-03-17
category: JavaScript
tags: JavaScript TypeScript
toc: 
  - name: Some Notes
  - name: Some Skills
---

## Some Notes

- JavaScript is single-threaded non-blocking asynchronous concurrent language. 

- JavaScript is all about Object! 

- JavaScript have a call stack, an event loop, a callback queue, and some other APIs. 
So JavaScript don’t just have JavaScript engine. 

- JavaScript is Dynamic language which means JavaScript do much more thing underneath. For example auto-boxing, auto type conversion, etc. And these make JavaScript harder to understand. You need to know what JavaScript will do for you.

- The class keyword is introduced in ES2015, but is syntactical sugar, JavaScript remains prototype-based. 

- About Inheritance, JavaScript only has one construct: objects. 
Inheritance is all about prototype chain: Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.

- When you get value from property, JS will look through prototype chain. When you Set value to property, JS will implement in own object, means if the property have been existed, then change value, if no, then create one. 

- In JavaScript, a primitive (primitive value, primitive data type) is data that is not an object and has no methods. There are 7 primitive data types: `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, and `null`.

- `undefined` vs `null`. Both of them are primitive data. `typeof(null)` will display `null` is object. But it is not. `null` is a deliberate assignment that represents no value. When a variable is declared but not initialized, or when a function does not return a value, the variable or the function’s result is `undefined`.
```js
null == undefined // true
null === undefined // false
```

- Except for null and undefined, all primitive values have object equivalents that wrap around the primitive values. The wrapper's valueOf() method returns the primitive value.

- In JavaScript, only objects and arrays are mutable. All primitive values are immutable.

## Some Skills

### avoid some code in Javascript:
```js
eval() 
arguments
for...in
with
delete
Hidden class
inline caching
```

### Use the Nullish Coalescing Operator (??) 

```js
function getUsername(name) {
  return name ?? "Guest";
}
console.log (getUsername("Ben")); // "Ben"
console.log (getUsername(null));  // "Guest"
console.log (getUsername(undefined)); // "Guest"
```
