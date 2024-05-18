---
layout: post
title: Callback in JS
date: 2021-06-10
categories: JavaScript
tags: Callback Event-loop Asynchronous JavaScript
toc: 
  - name: Callback Concept
  - name: Event handler
  - name: Callback Hell
---

## Callback Concept

The `setTimeout` function is a classic example to introduce callback concept. This function with the timeout ran after everything in the main top-level execution context. The timeout callback function can represent an asynchronous API call that contains data. 

Functions is **First-Class Objects** in JS. That means 
- They have built-in properties and methods, such as the name property and the .toString() method.
- Properties and methods can be added to them.
- They can be passed as arguments and returned from other functions.
- They can be assigned to variables, array elements, and other objects.

Any function can become a callback function if it is passed as an argument. Callbacks are not asynchronous **by nature**, but in asynchronous programming, like JS, callbacks usually refer to those which implement event loop, like function parameter in `setTimeout`.

A **higher-order function** is a function that accepts functions as parameters and/or returns a function.

### Example

Here is a syntactic code example of a higher-order function and a callback:
```javascript
// A function
function fn() {
  console.log('Just a function')
}

// A function that takes another function as an argument
function higherOrderFunction(callback) {
  // When you call a function that is passed as an argument, it is referred to as a callback
  callback()
}

// Passing a function
higherOrderFunction(fn)
```

In this code, you define a function fn, define a function higherOrderFunction that takes a function callback as an argument, and pass fn as a callback to higherOrderFunction.

Running this code will give the following:
```console
Just a function
```

## Event handler
An event handler is a particular type of callback. 
Event handlers are a form of asynchronous programming: you provide a function (the event handler) that will be called, not right away, but whenever the event happens. 
If "the event" is "the asynchronous operation has completed", then you could see how an event could be used to notify the caller about the result of an asynchronous function call.

> Some early asynchronous APIs used events in just this way.
> Like XMLHttpRequest API:
```javascript
  const xhr = new XMLHttpRequest();
  xhr.addEventListener('loadend', () => {
    log.textContent = `${log.textContent}Finished with status: ${xhr.status}`;
  });
  xhr.open('GET', 'https://raw.sample.com/wikihistory.json');
  xhr.send();
```
> Now, we use fetch() Api which is the modern, promise-based replacement for SMLHttpRequest. 

## Callback Hell
Callback functions are an effective way to ensure delayed execution of a function until another one completes and returns with data. However, due to the nested nature of callbacks, code can end up getting messy if you have a lot of consecutive asynchronous requests that rely on each other. This was a big frustration for JavaScript developers early on, and as a result code containing nested callbacks is often called the "pyramid of doom" or "callback hell."

Here is a demonstration of nested callbacks:

```javascript
function pyramidOfDoom() {
  setTimeout(() => {
    console.log(1)
    setTimeout(() => {
      console.log(2)
      setTimeout(() => {
        console.log(3)
      }, 500)
    }, 2000)
  }, 1000)
}
```

But, with real world asynchronous code, this can get much more complicated. You will most likely need to do error handling in asynchronous code, and then pass some data from each response onto the next request. Doing this with callbacks will make your code difficult to read and maintain.

Here is a runnable example of a more realistic "pyramid of doom" that you can play around with:
```javascript
// Example asynchronous function
function asynchronousRequest(args, callback) {
  // Throw an error if no arguments are passed
  if (!args) {
    return callback(new Error('Whoa! Something went wrong.'))
  } else {
    return setTimeout(
      // Just adding in a random number so it seems like the contrived asynchronous function
      // returned different data
      () => callback(null, { body: args + ' ' + Math.floor(Math.random() * 10) }),
      500
    )
  }
}

// Nested asynchronous requests
function callbackHell() {
  asynchronousRequest('First', function first(error, response) {
    if (error) {
      console.log(error)
      return
    }
    console.log(response.body)
    asynchronousRequest('Second', function second(error, response) {
      if (error) {
        console.log(error)
        return
      }
      console.log(response.body)
      asynchronousRequest(null, function third(error, response) {
        if (error) {
          console.log(error)
          return
        }
        console.log(response.body)
      })
    })
  })
}

// Execute
callbackHell()
```

Running this code will give you the following:
```console
First 9
Second 3
Error: Whoa! Something went wrong.
    at asynchronousRequest (<anonymous>:4:21)
    at second (<anonymous>:29:7)
    at <anonymous>:9:13
```

This way of handling asynchronous code is difficult to follow. As a result, the concept of promises was introduced in ES6. 
