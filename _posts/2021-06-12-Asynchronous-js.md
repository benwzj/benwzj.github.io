---
layout: post
title: "Asynchronous Programming in JS"
date: 2021-06-12
categories: JavaScript
tags: Call-back Promise Queue Asynchronous JavaScript
---

JavaScript is a single thread language. It use event loop to build Asynchronous programming.
Callbacks used to be the main way asynchronous functions were implemented in JavaScript. Now Promise is the main one.
Check another blogs for callback and promise concepts.

## async/await

### What is async/await
The async and await keywords make it easier to build an operation from a series of consecutive promises calls, avoiding the need to create explicit promise chains, and allowing you to write code that looks just like synchronous code. 

- Making it easier to write async code and easier to understand async code.

- async function is just the syntax sugar, which is build on top of promise.
The purpose of async/await is to simplify the syntax necessary to consume promise-based APIs. The behavior of async/await is similar to combining generators and promises.

### Features

- Async functions always return a promise. If the return value of an async function is not explicitly a promise, it will be implicitly wrapped in a promise.
```javascript
async function foo() {
   return 1
}
...is similar to:
function foo() {
   return Promise.resolve(1)
}
```
But they are not equivalent.
An async function will return a different reference, whereas Promise.resolve returns the same reference if the given value is a promise.

- async function run synchronously without await. But always run asynchronously with await. await can be put in front of any async promise-base function and pause your code on line until the promise fulfilled, then return the resulting value. 

- Code after each await expression can be thought of as existing in a .then callback.

- And await only works inside of async function.
		
- Promise.protoytype.then() block can be a long chain to promise-base methods. Now we can just add one await keyword before the the methods call. Just like a promise chain, await forces asynchronous operations to be completed in series. The return value forms the final link in the chain. 

### Common mistake when using async/await:
```javascript
async function fetchProducts() {
  const response = await fetch('https://mdn.github.io/products.json');
  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }
  const json = await response.json();
  return json;
}
const json = fetchProducts();
console.log(json[0].name);   // json is a Promise object, so this will not work
```
Changing last two lines will work:
```javascript
const jsonPromise = fetchProducts();
// Ben think it should use asyncPromise instead of jsonPromise as variable. 
// The whole async function is a promise. 
jsonPromise.then((json) => console.log(json[0].name));
```
But we usually use it like follow:
```javascript
async function fetchProducts() {
  const response = await fetch('https://mdn.github.io/products.json');
  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }
  const json = await response.json();
  console.log(json[0].name);
}
```

### Async functions and execution order

When there are many promise and many await in async function. The execution order will follow the promise.then order.
If want parallel the promise, need to use Promise.all().

Rewriting a Promise chain with an async function
Rewriting a Promise chain with async function will it easier to understand. 
```javascript
function getProcessedData(url) {
  return downloadData(url) // returns a promise
    .catch(e => {
      return downloadFallbackData(url)  // returns a promise
    })
    .then(v => {
      return processDataInWorker(v)  // returns a promise
    })
}
```
Rewrite to:
```javascript
async function getProcessedData(url) {
  const v = await downloadData(url).catch(e => {
    return downloadFallbackData(url)
  })
  return processDataInWorker(v)
}
```

## Worker

### What is Worker
Workers give you the ability to run some tasks in a different thread, so you can start the task, then continue with other processing (such as handling user actions).

- Worker is part of APIs which provided by runtime.
- Worker is wrote by JavaScript. Usually they are in different .js file.
- Workers and the main code run in completely separate worlds, and only interact by sending each other messages which are received by the other side as message events.
- Worker can't access all the APIs that the main application can, and in particular can't access the DOM.

- Workers may in turn spawn new workers, as long as those workers are hosted within the same origin as the parent page. 
- In addition, workers may use XMLHttpRequest for network I/O, with the exception that the responseXML and channel attributes on XMLHttpRequest always return null.

### Using Worker is very simple

A worker is an object created using a constructor (e.g. Worker()) that runs a named JavaScript file — this file contains the code that will run in the worker thread; 

Basic step:
1. In main.js
var myWorker = new Worker('worker.js');
myWorker.postMessage('Ali');
myWorker.addEventListener('message', message => {
  document.querySelector('#output').textContent = `${message.data}`;
});
2. In worker.js
addEventListener("message", message => {
  doSometing(message.data); //message.data is 'Ali'
  postMessage('Done with Ali');
});

### Q&A

- Is worker a part of JavaScript?
No, worker is web API. It run outside of the event loop, outside of the JavaScript Runtime.
