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

`setTimeout()` and `setInterval()` methods are very similiar, or you can say, same. 

## How it work underhood

`setTimeout()` is web API which means there are standards to define how it work. Becaue it need to make sure all browsers work consistently. Timers are described in the timers section of [HTML Living Standard](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html).

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
It is still called recursion, or called nested setTimeout. But their implement is different. 

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

## The `setInterval()`

The `setInterval()` function is commonly used to set a delay for functions that are executed again and again, such as animations. You can cancel the interval using `clearInterval()`.

Ensure that execution duration is shorter than interval frequency when using `setInterval()`. 
If there is a possibility that your logic could take longer to execute than the interval time, it is recommended that you recursively call a named function using `setTimeout()`. 

For example, if using `setInterval()` to poll a remote server every 5 seconds, network latency, an unresponsive server, and a host of other issues could prevent the request from completing in its allotted time. As such, you may find yourself with queued up XHR requests that won't necessarily return in order.

In these cases, a recursive `setTimeout()` pattern is preferred:
```js
(function loop() {
  setTimeout(() => {
    // Your logic here

    loop();
  }, delay);
})();
```
loop() is recursively called inside setTimeout() after the logic has completed executing. While this pattern does not guarantee execution on a fixed interval, it does guarantee that the previous interval has completed before recursing.

### What happen if not `clearInterval()`

## Common using way in React

- Using `ref` to mark intervalID. 
- `clearInterval(theOldInterval)` before start a new interval.

Example: building a stopwatch
```js
import { useState, useRef } from 'react';

export default function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    intervalRef.current = setInterval(() => {
      setNow(Date.now());
    }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>
        Start
      </button>
      <button onClick={handleStop}>
        Stop
      </button>
    </>
  );
}

```


## FQA

- Ensure that execution duration is shorter than interval frequency, using recursion of setTimeout() instead of setInterval()
- Working with asynchronous functions
- can be used in worker object becuase they are defined in a shared mixin (WindowOrWorkerGlobalScope)

## Reference

- [MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)
- [html.spec](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html)

