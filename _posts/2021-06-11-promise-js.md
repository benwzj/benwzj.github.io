---
layout: post
title: "Understand Promise in JS"
date: 2021-06-11
categories: JavaScript
tags: Promise Asynchronous JavaScript Microtask
---

## What is Promises

Firstly, Why Promise? It make using callback easier.

A promise represents the completion of an asynchronous function. It is an object that might return a value in the future. It accomplishes the same basic goal as a callback function, but with many additional features and a more readable syntax. 

### A Promise is in one of these states:
- pending: initial state, and keep in this state untill fulfilled or rejected.
- fulfilled: meaning that the operation completed successfully.
- rejected: meaning that the operation failed.

### Promise and thenable  
“promise” is an object or function with a then method whose behavior conforms to the specification.
[“thenable”](https://promisesaplus.com/) is an object or function that defines a then method.

A promise must provide a then method to access its current or eventual value or reason.
A promise’s then method accepts two arguments:
```javascript
promise.then(onFulfilled, onRejected)
```

A Promise is a proxy for a value not necessarily known when the promise is created. 
The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.

### I conclude some promise features: 
- We can define a Promise as an object that can produce a single value at some time in the future, either a value or the reason why it could not be resolved.
- A promise is an object returned by an asynchronous function, which represents the current state of the operation. 
- At the time the promise is returned to the caller, the operation often isn't finished, but the promise object provides methods to handle the eventual success or failure of the operation.
- A great feature is chaining. Need to have a good understanding of then().
- Promises are the foundation of asynchronous programming in modern JavaScript. 
- Compare to callback, a promise is a returned object to which you attach callbacks, instead of passing callbacks into a function.
- The main tricky thing of promise is that, it can resolve promise with a specific value, or another promise! 
- Promise VS old fashioned Callback, promise have some greater features: 
  - guarantees: 
A, Callback never call before current run; 
B, callback added with then(); 
C, multi callback can be added; 
  - chaining: call back can be called one by one by one...
  - chaining after catch: It can be chain after failure. Using catch().then()	


## Promise constructor

The Promise constructor is primarily used to wrap a function and make it as part of promise.
Write a promise constructor yourself, it is not what you imagine! The executor behavior is weird. Let's make it clearer step by step.

Promise constructor syntax: `new Promise(executor)`
You need to Understand executor.

### What is executor

`executor` is a function that is passed with TWO arguments: __resolve function__ and __reject function__. This two arguments were passed by `then()` function. They can’t be pass by function name or reference directly. 
The `executor` function is executed immediately by the Promise implementation, passing resolve and reject functions (the executor is called before the Promise constructor even returns the created object).
The `executor` is executed immediately. But `resolve` or `reject` function was not executed at the moment (because they still not pass in). All other code will executed. 

### Some main points about executor:
- If there are more than one resolve handler, it just queue the first one.
- About the executor, it's important to understand the following:
1. The executor return value is ignored.
2. If an error is thrown in the executor, the promise is rejected.

### Why ‘second and third time of resolve’ are missing in below code?
```javascript
const promise1 = new Promise((resolve, reject) => {
  console.log(start of executor')
  resolve ('first time of resolve');
  resolve ('second time of resolve');
  setTimeout(() => {
    resolve('third time of resolve');
  }, 1000);
  console.log('end of executor')
});

promise1.then((value) => {
  console.log(value);
});

> "start of executor"
> "end of executor"
> "first time of resolve"
```
Because there is mechanism by which the code within the executor has effect is as follows:

1. At the time when the constructor generates the new Promise object, it also generates a corresponding pair of functions for resolutionFunc and rejectionFunc; these are "tethered" to the Promise object.
2. The code within the executor has the opportunity to perform some operation and then reflect the operation's outcome (if the value is not another Promise object) as either "resolved" or "rejected", by terminating with an invocation of either the resolutionFunc or the rejectionFunc, respectively.
3. In other words, the code within the executor communicates via the side effect caused by resolutionFunc or rejectionFunc. The side effect is that the Promise object either becomes "resolved", or "rejected".

Briefly, what executor do is, 
1. tether two function with the created promise object. 
2. have opportunity to perform operation and to reflect outcome as resolved or rejected. 

### But why this one run like this...
```javascript
Let prom = new Promise(resolve => {
    setTimeout(function() {
      resolve("resolved now")
      console.log("this is ending")
    }, 2000)
  });
prom.then (message=>console.log(message));
> this is ending
> resolved now
```

### Summary of the executor typical flow:

1, The operation within executor is asynchronous and provides a callback.
2, The callback is defined within the executor code.
3, The callback terminates by invoking resolutionFunc.
4, The invocation of resolutionFunc includes a value parameter.
5, The value is passed back to the tethered Promise object.
6, The Promise object (asynchronously) invokes any associated .then(handleResolved).
7, The value received by .then(handleResolved) is passed to the invocation of handleResolved as an input parameter.

### Executor return value
When called via new, the Promise constructor returns a promise object. 
- The promise object will become "resolved" when either of the functions resolutionFunc or rejectionFunc are invoked. 
- Note that if you call resolutionFunc or rejectionFunc inside of the executor and pass another Promise object as an argument, you can say that it is "resolved", but still cannot be said to be "settled".

The executor normally initiates some asynchronous work, and then, once that completes, either calls the resolutionFunc to resolve the promise or call rejectionFunc to reject. (no matter which one you invoke, after the invocation, the executor terminated. You can just call one of them!)

### Basic Sample: 
```javascript
const myFirstPromise = new Promise((resolve, reject) => {
  // do something asynchronous which eventually calls either:
  //
  //   resolve(someValue)        // fulfilled
  // or
  //   reject("failure reason")  // rejected
});
 ```
### Simple basic example: 
```javascript
Let prom = new Promise(resolve => {
    setTimeout(function() {
      resolve("resolved now")
      console.log("this is ending")
    }, 2000)
  });
prom.then (message=>console.log(message));
> this is ending
> resolved now
```

### Provide a function with promise functionality, have it return a promise:
```javascript
function myAsyncFunction(url) {
  Return new Promise ((resolve, reject) => {
    const xhr = new XMLHttpRequest() 
    xhr.open("GET", url) 
    xhr.onload = () => resolve(xhr.responseText) 
    xhr.onerror = () => reject(xhr.statusText) 
    xhr.send() 
  });
}
```

## Understand then()
(`Promise.prototype.then()` is the **core** function of promise. )

`then()` itself returns a promise, which will be completed with the result of the handler function that was passed to it. 

It takes up to two arguments: callback functions for the success and failure cases of the Promise. And each callback function has a input argument! 
Syntax: 

```javascript
p.then(value => {
  // fulfillment
}, reason => {
  // rejection
});
```

### Rule of returned value from handler function
When a value is returned from within a then handler, it will effectively return `Promise.resolve(<value returned by whichever handler was called>)`.

There is a specific set of rules:
if handler ...
- returns a value, the promise returned by then gets resolved with the returned value as its value.
- returns another pending promise object, the resolution/rejection of the promise returned by then will be subsequent to the resolution/rejection of the promise returned by the handler. Also, the resolved value of the promise returned by then will be the same as the resolved value of the promise returned by the handler.
- returns an already fulfilled promise, the promise returned by then gets fulfilled with that promise's value as its value.
- returns an already rejected promise, the promise returned by then gets rejected with that promise's value as its value.
- doesn't return anything, the promise returned by then gets resolved with an undefined value.
- throws an error, the promise returned by then gets rejected with the thrown error as its value.

## Promise terminology

Terminologies include **pending**; **fulfilled**; **rejected, settled, resolved**.

This Promise terminology is come from this Wonderful Blog: [Let's talk about how to talk about promises](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/).

A promises's primary ***state*** is one of three mutually-exclusive values:
- **pending** - the initial state of most promises, it hasn't been fulfilled or rejected
- **fulfilled** - the promise has been fulfilled with a fulfillment value
- **rejected** - the promise has been rejected with a rejection reason (saying why the promise can't be fulfilled)

**"settled"** is the collective term which means **"fulfilled or rejected."**. 
Till now we know pending, fulfilled, rejected, settled. And they are easy to understand. But How about **"resolved"**.

### What is "resolved".
When you resolve a promise, you determine what will happen to that promise from then on. 
***"resolved"*** is NOT a state of a promise.

When you resolve a promise with something like `42` or `"answer"` or `{"example": "result"}`, yes, you do fulfill the promise with that value. But if you resolve your promise to another promise (or more generally a thenable), you're telling your promise to follow that other promise and do what it does:
- If the other promise is fulfilled, your original promise will fulfill itself with the other promise's fulfillment value
- If the other promise is rejected, your original promise will reject itself with the other promise's rejection reason
- If the other promise never settles, your original promise won't either

Regardless of what happens, though, there's nothing further you can do to the promise to affect the outcome. The promise is resolved to the other promise, irrevocably. Any attempt to resolve it again, or to reject it, will have no effect.

Now I try to make it simple to remember: A promise is resolved if it is settled, **OR** if it has been "locked in" to follow the state of another promise.
So, it is contrary to popular belief, resolving a promise doesn't necessarily change its primary state. In fact, it often doesn't. Promise resolution is a separate concept from promise fulfillment.

### "Why use the word 'resolved' when things are still up in the air?" 
It's because of the irrevocability: once a promise is resolved, nothing can change what's going to happen to it. If it's resolved with a non-promise value, it's fulfilled with that value and that's that. If it's resolved to a promise, it's going to follow that other promise and that's that. You can't change its resolution, or reject it directly. 

### Three ways to resolve a promise
1. calling the resolve function you get from new Promise and 
2. returning a value from a promise handler callback. 
3. A third way you resolve a promise is by using Promise.resolve. Promise.resolve creates a promise that's resolved to what you pass into it. One of its primary use cases is where you don't know what you're going to receive — a native promise, a non-native promise from a library like Q or jQuery, a thenable, or a non-thenable value. By passing any those through Promise.resolve and consuming the resulting promise, you can treat them all the same way:
```javascript
Promise.resolve(input)
.then(x => {
    // ...
})
```
### Understand what does the following code mean step by step
(assuming first() and second return will return promise): 
```javascript
function doStuff() {
    return first()
    .then(firstResult => {
        return second(firstResult);
    });
}
// ...
doStuff()
.then(result => {
    // ...use `result`...
});
.catch(error => {
    // ...handle/report error...
});
```
1. When you call doStuff, it calls first which creates and returns a promise (Promise A).
2. When you call then on that promise, then creates and returns another promise, the one that doStuff returns (Promise B).
3. Let's assume that at some point, the promise from first is fulfilled. The fulfillment handler in doStuff calls second with that fulfillment value and returns the promise second gives it (Promise C). That resolves Promise B to Promise C. From that point forward, Promise B follows Promise C and does what it does.

### Understand the async code, same issue with above code: 
```javascript
async function doStuff() {
    const firstResult = await first();
    return second(firstResult);
}
```

If the promise from first is fulfilled, doStuff calls second and then resolves its promise to the promise second returns. At that point, until/unless second's promise is settled, the promise from doStuff is both pending and resolved. It will fulfill itself, or reject itself, when/if second's promise settles.

## Promise and event loop

No Surprise, Promise use event loop, callback queue! 

First, Adding some concepts: there are microtasks and macrotasks in JS. And there are microtask queue and macrotask queue in JS. **Callbacks** of Promise objects will be microtasks. 

- The macrotasks, or just call tasks, which is any JavaScript scheduled to be run by the standard mechanisms such as initially starting to execute a program, an event triggering a callback, and so forth. Other than by using events (like onClick), you can enqueue a task by using setTimeout() or setInterval().

- The microtasks, which are programmed for things that should happen immediately after the script that is currently running, such as performing something asynchronous without supporting the penalty of creating a new macrotask. 
These microtasks are glued into the microtask queue which is processed after the macrotasks and at the end of the execution of each macrotask provided there is no Javascript running. Among the microtask are the callbacks of Promise objects.
If we add new microtasks to microtask queue during the execution of the microtasks, they are also executed.

As a corollary of this sequence we could say that two macrotasks cannot be executed one after the other if, in between, the microtasks tail has elements.

### example: 
```javascript
setTimeout(function() {   
  console.log('timeout); 
}, 0);
Promise.resolve()
  .then(function() {   
    console.log('promise'); 
  })
console.log('start');
// start
// promise
// timeout
```
Why promise run before timeout, because the script itself is treated as a macrotask so that at the end the enqueued microtasks are executed.

### task vs. microtask 

The difference between the task queue and the microtask queue is simple but very important:

- When executing tasks from the task queue, the runtime executes each task that is in the queue at the moment a new iteration of the event loop begins. Tasks added to the queue after the iteration begins will not run until the next iteration.
- Each time a task exits, and the execution context stack is empty, each microtask in the microtask queue is executed, one after another. The difference is that execution of microtasks continues until the queue is empty—even if new ones are scheduled in the interim. In other words, microtasks can enqueue new microtasks and those new microtasks will execute before the next task begins to run, and before the end of the current event loop iteration.

## Promise methods

### Promise.resolve(value)

Promise.resolve(value) returns a promise object, and the object is resolved with a given value:
1. if value is a promise, exact the same promise is returned;
2. if value is a thenable, the return promise will follow that thenable, adopting its eventual state;
3. otherwise, the return promise will be fulfilled with the value. 
```javascript		
Promise.resolve('Success').then (
	function(value) {
		console.log(value); // "Success"
	}, function(value) {
	   // not called
   }
);
```
Why we need Promise.resolve?
1. This function flattens nested layers of promise-like objects 
(e.g. a promise that resolves to a promise that resolves to something) into a single layer.
		
2. turn thenable (i.e. has a "then" method) object into Promise, the returned promise will "follow" that thenable, adopting its eventual state; otherwise the returned promise will be fulfilled with the value. 
```javascript
// Resolving a thenable object
var p1 = Promise.resolve({ 
	then: function(onFulfill, onReject) { onFulfill('fulfilled!'); }
});
console.log(p1 instanceof Promise) // true, object casted to a Promise
```

Promise constructor vs. Promise.resolve.
- They all return a promise.
- resolve() in following is in event queue.
```javascript
Promise.resolve().then (resolve); // this resolve will be put in microtask event queue.
```
- In constructor, the executor is called before the Promise constructor even returns the created object. __That means if you call resolve() directly in executor, the resolve() ignite synchronously.__ Oh no, it is wrong, that is tricky: executor is executed, except the resolve()! even you call resolve() directly. Resolve() is not ignite until .then(). It looks be put in microtask as well.

### Promise.prototype.catch()
In practice, it is often desirable to catch rejected promises rather than use then's two case syntax: 
```javascript
Promise.resolve()
  .then(() => {
    // Makes .then() return a rejected promise
    throw new Error('Oh no!');
  })
  .catch(error => {
    console.error('onRejected function called: ' + error.message);
  })
  .then(() => {
    console.log("I am always called even if the prior then's promise rejects");
  });.
```
in fact, calling obj.catch(onRejected) internally calls obj.then(undefined, onRejected)). 

### Promise.all()
Sometimes you need all the promises to be fulfilled, but they don't depend on each other. In a case like that it's much more efficient to start them all off together, then be notified when they have all fulfilled. The Promise.all() method is what you need here. It takes an array of promises, and returns a single promise.

The promise returned by Promise.all() is:
- fulfilled when and if all the promises in the array are fulfilled. In this case the then() handler is called with an array of all the responses, in the same order that the promises were passed into all()
- rejected when and if any of the promises in the array are rejected. In this case the catch() handler is called with the error thrown by the promise that rejected.

### Promise.any()
Sometimes you might need any one of a set of promises to be fulfilled, and don't care which one. In that case you want Promise.any().

### Promise.reject()  
is similar with Promise.resolve(). Returning a promise object which then() function just activate reject parameter. 

### Promise.race()
- It's parameter is An iterable object, such as an Array.
- Return value
A pending Promise that asynchronously yields the value of the first promise in the given iterable to fulfill or reject.
- example: 
```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one');
});
const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two');
});
Promise.race([promise1, promise2]).then((value) => {
  console.log(value); // expected output: "two" ; Both resolve, but promise2 is faster
});
```

## Promise conclusion
- Understand promise is a object. This object wraps the async code which still not completed when the object was created. The object provide callback functions handler which tethered with the async code.
- If you resolved promise A to promise B, then A follows B and does what it does. (actually at this point, promise A === promise B)
- One great feature of Promise is chaining. Simple promise chains are best kept flat without nesting, as nesting can be a result of careless composition.(But nesting limits the scope of inner error handlers, help catch error precisely)
- Promise constructor’s executor is a specific function, it works in different way from normal function.
- The passed-in function of the promise is put on microtasks queu, which means it runs later (only after the function which created it exits, and when the JavaScript execution stack is empty), just before control is returned to the event loop;

- Usually, in our daily promise usage is consume promise. 
- Using Promise constructor to implement promise.
- Miscrotask queue is the underneath support of promise. It enqueue all promise callbacks to miscrotask queue. 

## FQA

### What is the relationship between asynchronous programming and event loop.
In JavaScript, all asynchronous programming is running with event loop. They are using either macrotask queue or microtask queue.
For example, setTimeout() is using macrotask. Promise is using microtask.

### Why promise can solve callback hell problem?
promise chaining can flat ever-increasing levels of indentation when we need to make consecutive asynchronous function calls.

### callbacks vs. event loop?
If the callbacks don’t use event loop and callback queue, they are just synchronous. Your callback functions run in the same thread of your main function. 
For example defines two functions in JS and pass one function as parameter to another function and run. This kind of callback is synchronous. 
When your callbacks is APIs and run in other thread and it will setup your callback function into callback queue and event loop. Then it is async.  

### What is the thenable object
usually refer to the thenable object. Looks like:
```javascript
{
	then: function (resolve, error) {...} 
}
```
just like the then function. 

> Pay attention to thenable fucntion 's parameters: TWO of them: first one is success, second one is fail. Just like the constructor : new Promise(executor)
{: .block-warning }

Example: 
```javascript
// Promise.resolve() cast thenable object into a promise.
var p1 = Promise.resolve({ 
  then: function(onFulfill, onReject) { onFulfill('fulfilled!'); }
});
console.log(p1 instanceof Promise) // true, object casted to a Promise
		
// this promise object's then () function works following thenable object:
p1.then(function(v) {
  console.log(v); // "fulfilled!"
}, function(e) {
  // not called
});
```

### Promise and setTimeout, which first? promise! why?
```javascript
setTimeout(()=>{console.log('Timeout')}, 0)
Promise.resolve('promise').then((value)=>console.log(value));
console.log( 'now' );
// get: 
> "now"
> "promise"
> "Timeout"
```

### Did you know...
- that "fulfilling" a promise and "resolving" a promise aren't the same thing?
- that a promise can be both "pending" and "resolved" at the same time?
- that lots of your code is creating these pending resolved promises?
- that when you resolve a promise you might be rejecting it rather than fulfilling it (or neither)?