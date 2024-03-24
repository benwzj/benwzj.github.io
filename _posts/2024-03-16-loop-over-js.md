---
layout: post
title: Loop Over in JS
date: 2024-03-16
category: JavaScript
tags: JavaScript React
---

## for

The `for` statement creates a loop that consists of three optional expressions, enclosed in parentheses and separated by semicolons, followed by a statement (usually a block statement) to be executed in the loop.

```js
let str = '';

for (let i = 0; i < 9; i++) {
  str = str + i;
}
```

## for...of

The `for...of` statement executes a loop that operates on a sequence of values sourced from an **iterable** object. Iterable objects include instances of built-ins such as Array, String, TypedArray, Map, Set, NodeList (and other DOM collections), as well as the arguments object, generators produced by generator functions, and user-defined iterables.

When a `for...of` loop iterates over an iterable, it first calls the iterable's `[@@iterator]()` method, which returns an iterator, and then repeatedly calls the resulting iterator's `next()` method to produce the sequence of values to be assigned to variable.

## for...in

The `for...in` statement iterates over all enumerable string properties of an object (ignoring properties keyed by symbols), including inherited enumerable properties.

### `for...of` vs `for...in`

When loop over an Array, unlike `for...of`, `for...in` uses property enumeration instead of the array's iterator. In sparse arrays, `for...of` will visit the empty slots, but `for...in` will not.

## while
The while statement creates a loop that executes a specified statement as long as the test condition evaluates to true. The condition is evaluated before executing the statement.

```js
let n = 0;

while (n < 3) {
  n++;
}
```

## loop over an object

The prefered way is using `Object.entries()` or `Object.keys()` method. It is clean. 
```js
const object1 = {
  a: 'somestring',
  b: 42,
};
for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}
// Expected output:
// "a: somestring"
// "b: 42"
```

Alternative way: You can still use `for...in`. But it iterates through properties in the prototype chain. This means that we need to check if the property belongs to the object using `hasOwnProperty`.

## Loop Over Set

You can iterate over the elements of a Set using various methods, such as the `forEach()` method, the `for...of` loop, or by converting the Set to an array and using array iteration methods.

```ts
let mySet = new Set([1, 2, 3, 4, 5]);

mySet.forEach(function(value) {
  console.log(value);
});

```
Using `for...of` loop, (but old browser may not support):
```ts
let mySet = new Set([1, 2, 3, 4, 5]);

for (let value of mySet) {
  console.log(value);
}
```
## Reference

- [for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)
- [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)