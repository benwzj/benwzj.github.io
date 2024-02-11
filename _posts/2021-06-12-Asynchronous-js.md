---
layout: post
title: "Asynchronous Programming in JS"
date: 2021-06-12
categories: JavaScript
tags: Call-back Promise Asynchronous JavaScript
toc: 
  - name: async/await
  - name: Worker
  - name: FQA

---

Modern Asynchronous Programming is build the top of Callback.

## Synchronous vs. Asynchronous Callback

### DEFINITIONS

- A synchronous callback is invoked before a function returns, that is, while the API receiving the callback remains on the stack. An example might be: `list.foreach(callback)`; when `foreach()` returns, you would expect that the callback had been invoked on each element.
- An asynchronous or deferred callback is invoked after a function returns, or at least on another thread’s stack. Mechanisms for deferral include threads and main loops (other names include event loops, dispatchers, executors). Asynchronous callbacks are popular with IO-related APIs, such as `socket.connect(callback);` you would expect that when `connect()` returns, the callback may not have been called, since it’s waiting for the connection to complete.

### GUIDELINES

- A given callback should be either always sync or always async, as a documented part of the API contract.
- An async callback should be invoked by a main loop or central dispatch mechanism directly, i.e. there should not be unnecessary frames on the callback-invoking thread’s stack, especially if those frames might hold locks.

### HOW ARE SYNC AND ASYNC CALLBACKS DIFFERENT?

Sync and async callbacks raise different issues for both the app developer and the library implementation.

Synchronous callbacks:

- Are invoked in the original thread, so do not create thread-safety concerns by themselves.
- In languages like C/C++, may access data stored on the stack such as local variables.
- In any language, they may access data tied to the current thread, such as thread-local variables. For example many Java web frameworks create thread-local variables for the current transaction or request.
- May be able to assume that certain application state is unchanged, for example assume that objects exist, timers have not fired, IO has not occurred, or whatever state the structure of a program involves.

Asynchronous callbacks:

- May be invoked on another thread (for thread-based deferral mechanisms), so apps must synchronize any resources the callback accesses.
- Cannot touch anything tied to the original stack or thread, such as local variables or thread-local data.
- If the original thread held locks, the callback will be invoked outside them.
- Must assume that other threads or events could have modified the application’s state.

Neither type of callback is “better”; both have uses. Consider:
```js
list.foreach(callback)
```
in most cases, you’d be pretty surprised if that callback were deferred and did nothing on the current thread!
But:
```js
socket.connect(callback)
```
would be totally pointless if it never deferred the callback; why have a callback at all?

These two cases show why a given callback should be defined as either sync or async; they are not **interchangeable**, and don’t have the same purpose.

### CHOOSE SYNC OR ASYNC, BUT NOT BOTH
Not uncommonly, it may be possible to invoke a callback immediately in some situations (say, data is already available) while the callback needs to be deferred in others (the socket isn’t ready yet). The tempting thing is to invoke the callback synchronously when possible, and otherwise defer it. Not a good idea.

Because sync and async callbacks have different rules, they create different bugs. It’s very typical that the test suite only triggers the callback asynchronously, but then some less-common case in production runs it synchronously and breaks. (Or vice versa.)

Requiring application developers to plan for and test both sync and async cases is just too hard, and it’s simple to solve in the library: If the callback must be deferred in any situation, always defer it.

## Asynchronous Programming in JS
JavaScript is a single thread language. It use **Event Loop** to build Asynchronous programming. 
So you can said Event Loop is the core for Asynchronous Programming. Python use the similar way to implement Asynchronous.

[Callbacks](/blog/2021/callback-js/) used to be the main way asynchronous functions were implemented in JavaScript. Now [Promise](/blog/2021/promise-js/) is the main one.


## async/await

The `async` and `await` keywords make it easier to build an operation from a series of consecutive promises calls, avoiding the need to create explicit promise chains, and allowing you to write code that looks just like synchronous code. 

- Making it easier to write async code and easier to understand async code.
- async function is just the syntax sugar, which is build on top of promise.
The purpose of async/await is to simplify the syntax necessary to consume promise-based APIs. The behavior of async/await is similar to combining generators and promises.

### Features

- Async functions always return a promise. If the return value of an async function is not explicitly a promise, it will be implicitly wrapped in a promise.
```javascript
async function foo() {
   return 1
}
// ...is similar to:
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

## FQA

### Is worker a part of JavaScript Engine?
No, worker is web API. It run outside of the event loop, outside of the JavaScript Runtime.


## Reference

[Blog: Callbacks, synchronous and asynchronous](https://blog.ometer.com/2011/07/24/callbacks-synchronous-and-asynchronous/)