---
layout: post
title: JRE Overview
date: 2021-06-19
category: JavaScript
tags: JavaScript-Engine Web-API Event-loop
toc:
  - name: JavaScript Engine
  - name: The Event Loop
  - name: Web API
  - name: Callback Queue
---

JRE(JavaScript Runtime Environment) is what makes JavaScript code work.
The JRE provides access to built-in libraries and objects that are available to a program so that it can interact with the outside world and make the code work.

In the context of a browser this is comprised of the following 4 elements:
- The **JavaScript engine**(which in turn is made up of the heap and the call stack)
The JS engine translates source code into machine code that allows a computer to perform specific tasks at the hardware level
- **Web APIs**
Web APIs extend the JS language and push callback functions to the callback queue once actions are complete and data has been received
- The **callback queue**
The callback queue stores callback functions in order, ready to be executed.
- The **event loop**
The event loop is constantly monitoring the call stack and the callback queue; if the call stack is empty it will move the callback function at the front of the queue to the call stack, scheduling it for execution.

{% include figure.html path="assets/img/JRE.png" class="img-fluid rounded z-depth-1" width="70%" %}

## JavaScript Engine
A JavaScript engine is a software component that executes JavaScript code. 
- The first JavaScript engines were mere interpreters, but all relevant modern engines use just-in-time compilation for improved performance.

- There are 4 main JavaScript Engines: V8, SpiderMonkey, Chakra, JavaScriptCore. We will take V8 as example.

### JavaScript Engine Roughly looks as below:

{% include figure.html path="assets/img/JavaScript-engine.png" class="img-fluid rounded z-depth-1" width="70%" %}

### Call Stack
Also call Execution context stack (ECS).
This is where your stack frames are as your code executes.

- JavaScript is a single-threaded programming language, which means it has a single Call Stack. Therefore it can do one thing at a time.

- Execution context stack is a stack data structure which follows the Last In First Out (LIFO) principle. JavaScript engine always executes the function which is at the top of the execution context stack.

### Memory Heap 
This is where the memory allocation happens.
Heap is a large unstructured data structure which stores all the dynamic data like function definitions, objects, arrays etc. 

- Execution context stack just contains their reference of function, objects and arrays. The memory occupied in the heap continues to exist even after the JavaScript code execution has completed. They are removed by the JavaScript Garbage Collector.

### What happens when things slow down? 
If some job is too big and take time, then browser really slow down. Is programmer’s responsible to avoid this. They can use asynchronous callbacks.

### The V8 engine pipeline:

{% include figure.html path="assets/img/v8-pipeline.png" class="img-fluid rounded z-depth-1" width="70%" %}

## The Event Loop

JavaScript has a runtime model based on an event loop, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks. This model is quite different from models in other languages like C and Java.

A downside of this model is that if a message takes too long to complete, the web application is unable to process user interactions like click or scroll. 

### event loop vs. Multi thread
A very interesting property of the event loop model is that JavaScript, unlike a lot of other languages, never blocks. Handling I/O is typically performed via events and callbacks, so when the application is waiting for an IndexedDB query to return or an XHR request to return, it can still process other things like user input.

- In Event loop, Each message is processed completely before any other message is processed.

- This offers some nice properties when reasoning about your program, including the fact that whenever a function runs, it cannot be preempted and will run entirely before any other code runs (and can modify data the function manipulates). 

- This differs from C, for instance, where if a function runs in a thread, it may be stopped at any point by the runtime system to run some other code in another thread.

- A downside of this model is that if a message takes too long to complete, the web application is unable to process user interactions like click or scroll. 

## Web API

Web APIs are not part of the JS engine but they are part of the JavaScript Runtime Environment which is provided by the browser. JavaScript just provides us with a mechanism to access these API’s. As Web APIs are browser specific, they may vary from browser to browser. There may be cases where some Web APIs may be present in one browser but not in other.

Examples:

- **DOM API** for manipulating the DOM. document.getElementById,addEventListerner, document.querySelectorAll, etc. are part of the DOM API provided by the browser which we can access using JavaScript.
- **AJAX** calls or XMLHttpRequest. As Web APIs are browser specific and XMLHttpRequest is a Web API, we had to implement XMLHttpRequest in a different way for IE before JQuery saved us (remember?).
- **Timer** functions like setTimeout and setInterval are also provided by the browser.

When JavaScript engine finds any Web API method, it sends that method to an **event table**, where it waits till an event occurs. 
- In the case of AJAX calls, the JS engine will send the AJAX calls to the event table and will continue the execution of code after the Ajax call. AJAX call will wait in the event table until the response from the AJAX call is returned. 
- In case of timer function like setTimeout, it waits until the timer count becomes zero.


## Callback Queue

### What is Event Table

It is a table data structure to store the asynchronous methods which need to be executed when some event occurs.

### What is Callback Queue

Callback Queue is a queue data structure which follows First In First Out principle (item to be inserted first in the queue will be removed from the queue first). It stores all the messages which are moved from the event table to the event queue. Each message has an associated function. Callback queue maintains the order in which the message or methods were added in the queue.

Web API callbacks are moved from the event table to the event queue when an event occurs. For example, when AJAX calls are completed and the response is returned, it is moved from the event table to the event queue. Similarly, when the setTimout method waiting time becomes zero it is moved from the event queue to the event table.

