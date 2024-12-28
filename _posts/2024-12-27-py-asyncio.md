---
layout: post
title: Python Asyncio
date: 2024-12-27
categories: Python
tags: Python	Asynchronous
toc: 
  - name: What is asyncio
  - name: Coroutines
  - name: Task
  - name: Futures
  - name: Event loop
  - name: asynchronous context manager
  - name: Usage
  - name: FAQ
---

## What is asyncio

Asyncio is a built-in library of Python to write concurrent code using `async/await` syntax. This library provides high-performance network and web servers, database connection libraries, distributed task queues, etc., for asynchronous programming.

- asyncio is the concurrency module introduced in Python 3.4. 
- It is designed to use coroutines and futures to simplify asynchronous code and make it almost as readable as synchronous code as there are no callbacks.
- To understand sayncio, the key point is understand event loop, although somebody keep saying coroutine is the core feature.
- The asyncio package is a library to write concurrent code. However, async IO is not threading, nor is it multiprocessing. It is not built on top of either of these.
- async IO is a single-threaded, single-process design: it uses cooperative multitasking.
- async IO gives a feeling of concurrency despite using a single thread in a single process.
- By default asyncio runs in production mode. In order to ease the development asyncio has a debug mode.

### event loops, coroutines and futures concepts

- An **event loop** manages and distributes the execution of different tasks. It registers them and handles distributing the flow of control between them.
- **Coroutines** are special functions that work similarly to Python generators, on await they release the flow of control back to the event loop. A coroutine needs to be scheduled to run on the event loop, once scheduled coroutines are wrapped in Tasks which is a type of Future.
- **Futures** represent the result of a task that may or may not have been executed. This result may be an exception.

## Coroutines

### What is Coroutines

- Detail on PEP 492.
- Coroutines declared with the async/await syntax is the preferred way of writing asyncio applications.
- Example:

```py
async def main():
    print('hello')
    await asyncio.sleep(1)
    print('world')
```
- Python coroutines are awaitables and therefore can be awaited from other coroutines.
- Simply calling a coroutine will not schedule it to be executed. 
- To actually run a coroutine in the event loop, asyncio provides the following mechanisms:
  - asyncio.run()
  - asyncio.create_task()
  - asyncio.TaskGroup()

- The term “coroutine” can be used for two closely related concepts:
  - a coroutine function: an async def function;
  - a coroutine object: an object returned by calling a coroutine function.

- A coroutine is a specialized version of a Python generator function. 
- The keyword async def introduces a native coroutine.
- The keyword await passes function control back to the event loop.

#### Chaining Coroutines
A key feature of coroutines is that they can be chained together. (Remember, a coroutine object is awaitable, so another coroutine can await it.) This allows you to break programs into smaller, manageable, recyclable coroutines

### Coroutines is generators

- Coroutines are repurposed generators that take advantage of the peculiarities of generator methods.
- Old generator-based coroutines use yield from to wait for a coroutine result. Modern Python syntax in native coroutines simply replaces yield from with await as the means of waiting on a coroutine result. 
- The use of await is a signal that marks a break point. It lets a coroutine temporarily suspend execution and permits the program to come back to it later.

### Awaitable Object
- An awaitable object generally implements an `__await__()` method. 
- Note: The generator iterator objects returned from generators decorated with types.coroutine() are also awaitable, but they do not implement `__await__()`.

- We say that an object is an awaitable object if it can be used in an await expression. 
- Many asyncio APIs are designed to accept awaitables.
- There are three main types of awaitable objects: coroutines, Tasks, and Futures.

### Coroutine Obejct
Coroutine objects returned from async def functions are awaitable.

- A coroutine’s execution can be controlled by calling `__await__()` and iterating over the result.
- Coroutines have the methods which are analogous to those of generators. Like coroutine.send(value), coroutine.throw(value), coroutine.close(). 
- However, unlike generators, coroutines do not directly support iteration.

### Asynchronous Iterators and “async for”

Along with plain `async/await`, Python also enables async for to iterate over an asynchronous iterator. 

#### What is asynchronous iterator

- An asynchronous iterable is able to call asynchronous code in its iter implementation, and asynchronous iterator can call asynchronous code in its next method. 

- To support asynchronous iteration:
  - An object must implement an `__aiter__` method returning an asynchronous iterator object.
  - An asynchronous iterator object must implement an `__anext__` method returning an awaitable.
  - To stop iteration `__anext__` must raise a StopAsyncIteration exception.

- An example of asynchronous iterable:

```py
class AsyncIterable:
    def __aiter__(self):
        return self

    async def __anext__(self):
        data = await self.fetch_data()
        if data:
            return data
        else:
            raise StopAsyncIteration

    async def fetch_data(self):
        ...
```
- A new statement for iterating through asynchronous iterators is supported:

```py
async for TARGET in ITER:
    BLOCK
else:
    BLOCK2
```
which is semantically equivalent to:

```py
iter = (ITER)
iter = type(iter).__aiter__(iter)
running = True
while running:
    try:
        TARGET = await type(iter).__anext__(iter)
    except StopAsyncIteration:
        running = False
    else:
        BLOCK
else:
    BLOCK2
```

- The purpose of an asynchronous iterator is for it to be able to call asynchronous code at each stage when it is iterated over.

### Asynchronous Context Managers And “async with ”

- An asynchronous context manager is a context manager that is able to suspend execution in its `__aenter__` and `__aexit__` methods.
- Asynchronous context managers can be used in an `async with` statement.

## Task

- Task is a concept which interact with event loop.
- Tasks are one of the primary ways to interact with the event loop.
- Tasks wrap coroutines and track when they are complete.
- Tasks are used to run coroutines in event loops. If a coroutine awaits on a Future, the Task suspends the execution of the coroutine and waits for the completion of the Future. When the Future is done, the execution of the wrapped coroutine resumes.
- Tasks are used to schedule coroutines concurrently.
- When a coroutine is wrapped into a Task with functions like asyncio.create_task() the coroutine is automatically scheduled to run soon.

### Create task
asyncio.create_task(coro)
- Wrap the coro coroutine into a Task and schedule its execution. Return the Task object.
- The task is executed in the loop returned by get_running_loop()
- Save a reference to the result of this function, to avoid a task disappearing mid-execution. 
- asyncio.TaskGroup.create_task() is a newer alternative that allows for convenient waiting for a group of related tasks.

#### Still have no idea why need task to wrap a coroutine?

OK, let’s clear it. 
- Firstly, coroutine just work within a event loop. You can just simply use asyncio.run(coro) to creates a new event loop to run coro and closes it at the end. 
- When you are using asyncio.gather() for concurrency, it actually use task.
- Using task provide more control, more features. For example, according code below:

```py
async def say_hello():
    await asyncio.sleep(2)
    print("Hello")

async def not_task():
    print(f"started at {time.strftime('%X')}")
    await say_hello()
    await say_hello()
    print(f"started at {time.strftime('%X')}")

async def is_task():
    t1 = asyncio.create_task(say_hello())
    t2 = asyncio.create_task(say_hello())
    print(f"started at {time.strftime('%X')}")
    await t1
    await t2
    print(f"started at {time.strftime('%X')}")
```
- understand HOW not_task() and is_task() work when wrap them in run().
Running not_task() coroutine object, it take 4 seconds; Running is_task(), it can run concurrently, it take 2 seconds.

### Task Groups
- New at version 3.11
- Task groups combine a task creation API with a convenient and reliable way to wait for all tasks in the group to finish.

## Futures

- A Future is a special low-level awaitable object that represents an eventual result of an asynchronous operation.
- Future objects are used to bridge low-level callback-based code with high-level async/await code.
- When a Future object is awaited it means that the coroutine will wait until the Future is resolved in some other place.
- Future objects in asyncio are needed to allow callback-based code to be used with async/await.
- Normally there is no need to create Future objects at the application level code.
- Future objects, sometimes exposed by libraries and some asyncio APIs, can be awaited.

## Event loop

- The event loop is the core of every asyncio application that takes care of all the running tasks. 
- The event loop supports multitasking. When a function is suspended, control returns to the loop, which then finds another function to start or resume.
- Event loops run asynchronous tasks and callbacks, perform network IO operations, and run subprocesses.
- Event loops use cooperative scheduling: an event loop runs one Task at a time. While a Task awaits for the completion of a Future, the event loop runs other Tasks, callbacks, or performs IO operations.
- Event loop interface provide low-level APIs. Application developers should rarely need to reference the loop object or call its methods. 
- By default, an async IO event loop runs in a single thread and on a single CPU core.
- Event loops are pluggable. That is, you could, if you really wanted, write your own event loop implementation and have it run tasks just the same. 


{% include figure.html path="assets/img/py-eventloop.jpg" class="img-fluid rounded z-depth-1" width="60%" %}

### How event loop work

- An event loop runs in a thread (typically the main thread) and executes all callbacks and Tasks in its thread. 
- While a Task is running in the event loop, no other Tasks can run in the same thread. 
- When a Task executes an await expression, the running Task gets suspended, and the event loop executes the next Task.
- To schedule a callback from another OS thread, the loop.call_soon_threadsafe() method should be used.

### high-level asyncio functions

#### asyncio.run(coro)
- asyncio.run() introduced in Python 3.7, is responsible for getting the event loop, running tasks until they are marked as complete, and then closing the event loop.
- It runs the passed coro, taking care of managing the asyncio event loop and closing the threadpool.
- This function cannot be called when another asyncio event loop is running in the same thread.
- It should be used as a main entry point for asyncio programs, and should ideally only be called once.
- There’s a more long-winded way of managing the asyncio event loop, with get_event_loop().

### low-level functions

Application developers should typically use the high-level asyncio functions, such as asyncio.run(), and should rarely need to reference the loop object or call its methods.

#### asyncio.get_running_loop()
- Return the running event loop in the current OS thread.
- Raise a RuntimeError if there is no running event loop.
- This function can only be called from a coroutine or a callback.

#### asyncio.get_event_loop()
- When called from a coroutine or a callback, this function will always return the running event loop. 
- If there is no running event loop set, the function will return the result of the get_event_loop_policy().get_event_loop() call.
- Consider using the higher-level asyncio.run() function, instead of using these lower level functions to manually create and close an event loop.

#### asyncio.set_event_loop(loop)
Set loop as the current event loop for the current OS thread.

#### asyncio.new_event_loop()
Create and return a new event loop object.

### Event Loop Methods

#### Running and stopping the loop
- `loop.run_until_complete(future)`: 
Run until the future (an instance of Future) has completed.

- `loop.run_forever()`: 
Run the event loop until stop() is called.

- `loop.close()`: 
Close the event loop.

#### Scheduling callbacks
- `loop.call_soon(callback, *args, context=None)`: 
Schedule the callback callback to be called with args arguments at the next iteration of the event loop.
Callbacks are called in the order in which they are registered. Each callback will be called exactly once.
- `loop.call_soon_threadsafe(callback, *args, context=None)`: 
When scheduling callbacks from another thread, this function must be used.
- `loop.call_later(delay, callback, *args, context=None)`: 
Schedule callback to be called after the given delay number of seconds.
- `loop.call_at(when, callback, *args, context=None)`: 
Schedule callback to be called at the given absolute timestamp when (an int or a float), using the same time reference as loop.time().
- `loop.time()`: 
Return the current time, as a float value, according to the event loop’s internal monotonic clock.

#### Creating Futures and Tasks

- `loop.create_future()`
Create an asyncio.Future object attached to the event loop.

This is the preferred way to create Futures in asyncio. This lets third-party event loops provide alternative implementations of the Future object 

- `loop.create_task(coro, *, name=None, context=None)`
Schedule the execution of coroutine coro. Return a Task object.

#### Opening network connections
- coroutine loop.create_connection()
Open a streaming transport connection to a given address specified by host and port.
- coroutine loop.create_datagram_endpoint()
Create a datagram connection.
- coroutine loop.create_unix_connection()

#### Creating network servers
- coroutine loop.create_server()
Create a TCP server (socket type SOCK_STREAM) listening on port of the host address.

## asynchronous context manager

An asynchronous context manager is a context manager that is able to suspend execution in its __aenter__ and __aexit__ methods.

### Runner context manager
`class asyncio.Runner(*, debug=None, loop_factory=None)`

- A context manager that simplifies multiple async function calls in the same context.
- Sometimes several top-level async functions should be called in the same event loop and contextvars.Context.
- Basically, asyncio.run() example can be rewritten with the runner usage:

```py
async def main():
    await asyncio.sleep(1)
    print('hello')

with asyncio.Runner() as runner:
    runner.run(main())
```

### coroutine asyncio.timeout(delay)
An asynchronous context manager that can be used to limit the amount of time spent waiting on something.

```py
async def main():
    try:
        async with asyncio.timeout(10):
            await long_running_task()
    except TimeoutError:
        print("The long operation timed out, but we've handled it.")

    print("This statement will run regardless.")
```

## Usage
### Sleeping
`coroutine asyncio.sleep(delay)`
Example, wait 1 second:
`await asyncio.sleep(1)`

### Running Tasks Concurrently
`awaitable asyncio.gather(*aws)`
- Run awaitable objects in the aws sequence concurrently.
- If any awaitable in aws is a coroutine, it is automatically scheduled as a Task.

### Task groups
- New in version 3.11
- Task groups combine a task creation API with a convenient and reliable way to wait for all tasks in the group to finish.
- `class asyncio.TaskGroup`
An asynchronous context manager holding a group of tasks. Tasks can be added to the group using TaskGroup.`create_task()`. All tasks are awaited when the context manager exits.

Example:
```py
async def main():
    async with asyncio.TaskGroup() as tg:
        task1 = tg.create_task(some_coro(...))
        task2 = tg.create_task(another_coro(...))
    print("Both tasks have completed now.")
```
The async with statement will wait for all tasks in the group to finish. 

Other topics:
- Shielding From Cancellation
- Timeouts
- Waiting Primitives
- Running in Threads
- Scheduling From Other Threads

## FAQ

- Asynchronous programming and Generator
- Are all coroutines which are in the same asyncio.run() are in a same thread? One event loop locate in one thread?
- The current thread which execute asyncio.run() is other than the event loop thread?

#### What is Concurrent computing
Concurrent computing is a form of computing in which several computations are executed concurrently—during overlapping time periods—instead of sequentially—with one completing before the next starts.

Concurrency is not Parallelism 
- Concurrency and parallelism, a related but quite distinct concept. 
- In programming, concurrency is the composition of independently executing processes, while parallelism is the simultaneous execution of (possibly related) computations. 
- Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.
- Concurrency is talk about structure, while parallelism is about execution. 
- Concurrency maybe use parallelism, but parallelism is not it’s goal. 
- An analogue: OS need to handle many stuff, keyboard, mouse input,screen output, response many socket. But it don’t have to be parallelism. it use concurrent model. 
- Parallelism, for example, break a big job into many small jobs and execute them in parallel way. 

#### @asyncio.coroutine
async/await is new syntax. 
@asyncio.coroutine return a generator-based coroutine which outdated.

Example:
```py
@asyncio.coroutine
def py34_coro():
    """Generator-based coroutine, older syntax"""
    yield from stuff()
same with:
async def py35_coro():
    """Native coroutine, modern syntax"""
    await stuff()
```
