---
layout: post
title: Understand Event Loop in JS
date: 2021-06-01
categories: JavaScript
tags: Event-loop JavaScript
toc:
  - name: Demo Example
  - name: Stack
  - name: Queue
---

JavaScript has a runtime model based on an event loop, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks. This model is quite different from models in other languages like C and Java.

A very interesting property of the event loop model is that JavaScript, unlike a lot of other languages, never blocks. Handling I/O is typically performed via events and callbacks, so when the application is waiting for an IndexedDB query to return or an XHR request to return, it can still process other things like user input.

## Demo Example

JavaScript code execute in a synchronous manner if no asynchronous Web API is used. This is demonstrated by this example code that calls three functions that each print a number to the console:
```javascript
// Define three example functions
function first() {
  console.log(1)
}

function second() {
  console.log(2)
}

function third() {
  console.log(3)
}
```
In this code, you define three functions that print numbers with console.log().

Next, write calls to the functions:

```javascript
// Execute the functions
first()
second()
third()
```
The output will be based on the order the functions were called: first(), second(), then third().
```
1
2
3
```
When an asynchronous Web API is used, the rules become more complicated.
Add setTimeout to the second function to simulate an asynchronous request:
```javascript
// Define three example functions, but one of them contains asynchronous code
function first() {
  console.log(1)
}

function second() {
  setTimeout(() => {
    console.log(2)
  }, 0)
}

function third() {
  console.log(3)
}
```

Now call the functions, as you did before:
```javascript
// Execute the functions
first()
second()
third()
```
You might expect with a setTimeout set to 0 that running these three functions would still result in the numbers being printed in sequential order. But because it is asynchronous, the function with the timeout will be printed last:
```
1
3
2
```
This happens because the JavaScript host environment, in this case the browser, uses a concept called the event loop to handle concurrency, or parallel events. Since JavaScript can only execute one statement at a time, it needs the event loop to be informed of when to execute which specific statement. The event loop handles this with the concepts of a stack and a queue.

## Stack

The stack, or call stack, holds the state of what function is currently running. If you're unfamiliar with the concept of a stack, you can imagine it as an **array** with "Last in, first out" (LIFO) properties, meaning you can only add or remove items from the end of the stack. JavaScript will run the current frame (or function call in a specific environment) in the stack, then remove it and move on to the next one.

For the example only containing synchronous code, the browser handles the execution in the following order:

- Add first() to the stack, run first() which logs 1 to the console, remove first() from the stack.
- Add second() to the stack, run second() which logs 2 to the console, remove second() from the stack.
- Add third() to the stack, run third() which logs 3 to the console, remove third() from the stack.

The second example with setTimout looks like this:

- Add first() to the stack, run first() which logs 1 to the console, remove first() from the stack.
- Add second() to the stack, run second().
  - Add setTimeout() to the stack, run the setTimeout() Web API which starts a timer and adds the anonymous function to the queue, remove setTimeout() from the stack.
- Remove second() from the stack.
- Add third() to the stack, run third() which logs 3 to the console, remove third() from the stack.
- The event loop checks the queue for any pending messages and finds the anonymous function from setTimeout(), adds the function to the stack which logs 2 to the console, then removes it from the stack.

Using setTimeout, an asynchronous Web API, introduces the concept of the queue, which this tutorial will cover next.

## Queue

The queue, also referred to as message queue or task queue, is a waiting area for functions. Whenever the call stack is empty, the event loop will check the queue for any waiting messages, starting from the oldest message. Once it finds one, it will add it to the stack, which will execute the function in the message.

In the setTimeout example, the anonymous function runs immediately after the rest of the top-level execution, since the timer was set to 0 seconds. It's important to remember that the timer does not mean that the code will execute in exactly 0 seconds or whatever the specified time is, but that it will add the anonymous function to the queue in that amount of time. This queue system exists because if the timer were to add the anonymous function directly to the stack when the timer finishes, it would interrupt whatever function is currently running, which could have unintended and unpredictable effects.

> ##### Note
>
> There is also another queue called the job queue or microtask queue that  handles promises. Microtasks like promises are handled at a higher priority than macrotasks like setTimeout.
{: .block-warning }

Now you know how the event loop uses the stack and queue to handle the execution order of code. The next task is to figure out how to control the order of execution in your code. To do this, you will first learn about the original way to ensure asynchrnous code is handled correctly by the event loop: callback functions.

