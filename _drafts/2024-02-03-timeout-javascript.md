---
layout: post
title: About Timer in JavaScript
date: 2024-02-03
category: JavaScript
tags: Web-API Timer
---

The `setTimeout()` and `setInterval()` methods allow authors to schedule timer-based callbacks.
I am not gonna talk about how to use it. But trying to dig a bit deepper.

`setTimeout()` and `setInterval()` methods arevery similiar, or can say, same.

- How it work underhood
- common using way in React
- What happen if not clearInterval();
- Ensure that execution duration is shorter than interval frequency, using recursion of setTimeout() instead of setInterval()
- This API does not guarantee that timers will run exactly on schedule. Delays due to CPU load, other tasks, etc, are to be expected.
- The "this" problem
- Working with asynchronous functions
- can be used in worker object becuase they are defined in a shared mixin (WindowOrWorkerGlobalScope)
- settimeout and recursion


## Understand the execution order

```js
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait(0).then(() => console.log(4));
setTimeout(()=>console.log(5),0);
Promise.resolve().then(() => console.log(2)).then(() => console.log(3));
console.log(1); 

// When execute this script, the order of log will be: "1, 2, 3, 4, 5"
```
Promise and setTimeout() both are using event loop, callback queue. But they have some difference.

### microtask, macrotask concept

There are **microtask queue** and **macrotask queue** in JS. 
Callbacks of Promise objects will be microtasks. 

- The macrotasks, or just call tasks, which is any JavaScript scheduled to be run by the standard mechanisms such as initially starting to execute a program, an event triggering a callback, and so forth. Other than by using events (like `onClick`), you can enqueue a task by using `setTimeout()` or `setInterval()`.

- The microtasks, which are programmed for things that should happen immediately after the script that is currently running, such as performing something asynchronous without supporting the penalty of creating a new macrotask. 
These microtasks are **glued** into the microtask queue which is processed after the macrotasks and at the end of the execution of each macrotask provided there is no Javascript running. Among the microtask are the callbacks of Promise objects.
If we add new microtasks to microtask queue during the execution of the microtasks, they are also executed.

So, As a corollary of this sequence we could say that two macrotasks cannot be executed one after the other if, in between, the microtasks tail has elements.


## Reference

[MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)

