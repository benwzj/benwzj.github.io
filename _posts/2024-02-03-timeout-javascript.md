---
layout: post
title: About Timer in JavaScript
date: 2024-02-03
category: JavaScript
tags: Web-API Timer Recursion
---

The `setTimeout()` and `setInterval()` methods allow authors to schedule timer-based callbacks.

- Timers can be nested; after five such nested timers, however, the interval is forced to be at least four milliseconds.
- This API does not guarantee that timers will run exactly on schedule. Delays due to CPU load, other tasks, nested level, inactive tab, etc, are to be expected.
- Non-number `delay` values are silently coerced into numbers.
- You can include a string instead of a function, which is compiled and executed when the timer expires. This syntax is not recommended for the same reasons that make using `eval()` a security risk.

I am not gonna talk about how to use it. But trying to dig a bit deepper.

`setTimeout()` and `setInterval()` methods are very similiar, or you can say, same. So I will focus on `setTimeout()`.

## How it work underhood

`setTimeout()` is web API which means there are standards to define how it work. Becaue it need to make sure all browsers work consistently.

`setTimeout()`s are the functions (a subset) that are common to all workers and to the main thread. They are from `WindowOrWorkerGlobalScope`. Objects that implement the `WindowOrWorkerGlobalScope` mixin have a map of active timers, which is a map, initially empty. Each key in this map is an identifier for a timer, and each value is a `DOMHighResTimeStamp`, representing the expiry time for that timer.

`setTimeout()` will run the [timer initialization steps](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timer-initialisation-steps) and put entries into the map of active timers.
In completionStep, an algorithm step, which **queues a global task** on the timer task source given global to run task.

## The "this" problem

Code executed by setTimeout() is called from an execution context separate from the function from which setTimeout was called. 
If you have not set `this` in the call or with bind, it will default to the window (or global) object. It will not be the same as the `this` value for the function that called setTimeout.

Solutions:
- A common way to solve the problem is to use a wrapper function that sets `this` to the required value.
- Alternatively, you can use bind() to set the value of this for all calls to a given function

## setTimeout and Recursion

The act of a function calling itself, recursion is used to solve problems that contain smaller sub-problems. A recursive function can receive two inputs: a base case (ends recursion) or a recursive case (resumes recursion).

To understand Recursion, you need to understand closure, call stack, etc. 

In JavaScript, Recursion is limited by stack size. It is easy to explode call stack.
For example: Function below returns the maximum size of the call stack available in the JavaScript runtime in which the code is run.

```js
const getMaxCallStackSize = (i) => {
  try {
    return getMaxCallStackSize(++i);
  } catch {
    return i;
  }
};

console.log(getMaxCallStackSize(0));
```

But if call function itself in setTimeout(), it is still Recursion? just like below:
```js
let count = 10;
const timer = ()=>{
  count --;
  let currentTime = new Date().getMilliseconds();
  if (count > 0){
    setTimeout (()=>{
      let executionTime = new Date().getMilliseconds();
      writeLog(currentTime, executionTime);
      timer();
    }, 0);
  }
}
```
It is still call recursion. But their implement is different. 

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

## FQA

- common using way in React
- What happen if not clearInterval();
- Ensure that execution duration is shorter than interval frequency, using recursion of setTimeout() instead of setInterval()
- Working with asynchronous functions
- can be used in worker object becuase they are defined in a shared mixin (WindowOrWorkerGlobalScope)

## Reference

- [MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)
- [html.spec](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html)

