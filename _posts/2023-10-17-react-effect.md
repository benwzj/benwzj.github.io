---
layout: post
title: Effect in React
date: 2023-10-17
tags: React Hook
category: React
toc:
  - name: What are Effects
  - name: Understand Effect
  - name: useEffect
    subsections: 
      - name: useEffect syntax
      - name: Common Patterns for Effect
      - name: Classic No Effect Cases
      - name: Classic Bugs - race conditions
  - name: FQA
---

Some components need to stay connected to the **network**, some **browser API**, or a **third-party library** while they are displayed on the page. These systems aren’t controlled by React, so they are called external.
How React Synchronize With External? React provide Effect system!

But always keep this in mind: Effects are an escape hatch from the React paradigm. Don't use it unless you have to.

## What are Effects

When talking about Effect, usually compare to Event. 

> ##### The definition
> 
> Effects let you specify side effects that are caused by rendering itself, rather than by a particular event. 
{: .block-warning }

> Capitalized “Effect” refers to the React-specific definition above, i.e. a side effect caused by rendering. To refer to the broader programming concept, we’ll say “side effect”.

Effects let you run some code after rendering so that you can synchronize your component with some system outside of React. 

## Understand Effect

To understand Effect, you need to be familiar with two types of logic inside React components:
1. **Rendering code** lives at the top level of your component. This is where you take the props and state, transform them, and return the JSX you want to see on the screen. Rendering code must be pure.
2. **Event handlers** are nested functions inside your components that do things rather than just calculate them. Event handlers contain “side effects” (they change the program’s state) caused by a specific user action (for example, a button click or typing).

Beside these two basic logic, React provide Effect logic. It is not on Rendering code or Event handlers. Effect is caused by rendering, and run after rendering. 
This logic is just designed for Synchronizing with external.

If you’re not connecting to any external system, you probably don’t need an Effect.

To declare an Effect in your component, you need to import the `useEffect` Hook from React.

## useEffect

`useEffect` is a React Hook. It lets you synchronize a component with an external system.

Every time your component renders, React will update the screen and then run the code inside `useEffect`. In other words, `useEffect` “delays” a piece of code from running until that render is reflected on the screen.

Each render has its own Effects. You can even think of `useEffect` as “attaching” a piece of behavior to the render output. 

> When think about using useEffect, One rule need to be consider first: 
Code that runs because a component was displayed should be in Effects, the rest should be in events.
{: .block-warning}

### useEffect syntax

`useEffect(setup, dependencies?)`

- `setup`: The function with your Effect’s logic. Your setup function may also optionally return a cleanup function.
- optional `dependencies`: The list of all reactive values referenced inside of the setup code. Reactive values include props, state, and all the variables and functions declared directly inside your component body.
1. If you specify the dependencies, your Effect runs after the initial render and after re-renders with changed dependencies.
2. If you omit this argument, your Effect will re-run after every re-render of the component.
3. If dependencies is `[]`, it will only run after the initial render. 

Optional `dependencies` rules:
```js
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

You can’t “choose” your `dependencies`. They are determined by the code inside the Effect.
React will call your `cleanup function` before the Effect runs next time, and during the unmount.

### Common Patterns for Effect

In Strict Mode, Effect will be run twice. That is helpful for development. For example, it can tell you that the Effect need `cleanup function` or not.

Most of the Effects you’ll write will fit into one of the common patterns below:

- Controlling non-React widgets 
Sometimes you need to add UI widgets that aren’t written to React. For example, let’s say you’re adding a map component to your page. It has a `setZoomLevel()` method, and you’d like to keep the zoom level in sync with a zoomLevel state variable in your React code.
- Subscribing to events 
If your Effect subscribes to something, the cleanup function should unsubscribe.
- Triggering animations 
If your Effect animates something in, the cleanup function should reset the animation to the initial values.
- Fetching data 
If your Effect fetches something, the cleanup function should either abort the fetch or ignore its result. Please note, Fetching data in Effect is not an elegant way. If you use a framework, use its built-in data fetching mechanism. Otherwise, consider using or building a client-side cache. 
- Sending analytics
- (Not an Effect): Initializing the application 
Some logic should only run once when the application starts. You can put it outside your components.

### Classic No Effect Cases 

In normal data flow, you should calculate what you can during rendering, and adjust the state in the event handler. Effect is just escape hatch, and it should always be last choice.
In Strict Mode, Effect will be run twice. This is a warning sign to tell whether your code should run in effect.

- Updating state based on props or state.
When something can be calculated from the existing props or state, don’t put it in state, don't use effect. Instead, calculate it during rendering.

- Caching expensive calculations
You can cache (or “memoize”) an expensive calculation by wrapping it in a `useMemo` Hook.

- Resetting all state when a prop changes.
By passing a new value as a `key` prop to the component, you’re asking React to treat two components with new `key` value as two different components that should not share any state. React will reset all state.

- Adjusting some state when a prop changes.
No matter how you do it, adjusting state based on props or other state makes your data flow more difficult to understand and debug. Always check whether you can reset all state with a key or calculate everything during rendering instead. 
An escape hatch is Storing information from previous renders. It is hard to understand, but it’s better than updating the same state in an Effect. 

- Sharing logic between event handlers.
When you’re not sure whether some code should be in an Effect or in an event handler, ask yourself why this code needs to run. Use Effects only for code that should run because the component was displayed to the user.

- Sending a POST request.
The analytics POST request should remain in an Effect. This is because the reason to send the analytics event is that the form was displayed. But you should know that most POST request is not caused by the form being displayed.

- Chains of computations 

- Initializing the application
Some logic should only run once when the app loads.
You can run it during module initialization and before the app renders:
```js
if (typeof window !== 'undefined') { // Check if we're running in the browser.
   // ✅ Only runs once per app load
  checkAuthToken();
  loadDataFromLocalStorage();
}

function App() {
  // ...
}
```
or like below:
```js
let didInit = false;
function App() {
  useEffect(() => {
    if (!didInit) {
      didInit = true;
      // ✅ Only runs once per app load
      loadDataFromLocalStorage();
      checkAuthToken();
    }
  }, []);
  // ...
}
```

- Notifying parent components about state changes
Usual way is calling the `onChange` function passed from a parent componen. Do this in event handler instead Effect.

- Passing data to the parent 
This is a anti-pattern design. You should design your app which get data in perant and pass to child. It will be easier to trace and debug.

- Subscribing to an external store
Although it’s common to use Effects for this, React has a purpose-built Hook for subscribing to an external store that is preferred instead. Delete the Effect and replace it with a call to `useSyncExternalStore`. Get more info form [here](https://react.dev/reference/react/useSyncExternalStore).

- Fetching data
While component is visible, you want to keep states synchronized with data from the network for the current page and query. You can use an Effect. However, you need to **add a cleanup function** to ignore _stale_ responses, e.g. using `ignore` flag. 
Also, you should think about caching when fetching data. It will make this logic more complicated. These issues apply to any UI library, not just React. Solving them is not trivial, which is why modern **frameworks** provide more efficient built-in data fetching mechanisms than fetching data in Effects.

In some cases, you can’t calculate the next state directly in the event handler. For example, imagine a form with multiple dropdowns where the options of the next dropdown depend on the selected value of the previous dropdown. Then, a chain of Effects is appropriate because you are synchronizing with network.

In general, whenever you have to resort to writing Effects, keep an eye out for when you can extract a piece of functionality into a custom Hook with a more declarative and purpose-built API like useData above. The fewer raw useEffect calls you have in your components, the easier you will find to maintain your application.

### Classic Bugs - race conditions 

When fetch data from internet in your `useEffect` according to user input, it is easy to cross classic bug race condition. You can fix this by adding flag `ignore`, like this:
```js
useEffect(() => {
  let ignore = false;
  setBio(null);
  fetchBio(person).then(result => {
    if (!ignore) {
      setBio(result);
    }
  });
  return () => {
    ignore = true;
  }
}, [person]);
```

Each render’s Effect has its **own** `ignore` variable. Initially, the `ignore` variable is set to false. However, if an Effect gets cleaned up (such as when you select a different person), its `ignore` variable becomes true. So now it doesn’t matter in which order the requests complete. Only the last person’s Effect will have `ignore` set to false, so it will call `setBio(result)`. 

The keys to understand this race conditions' resolusion are:
- Clear the design of effect's `cleanup function`.
- Clear asynchronous pattern and closure pattern in javascript.

## FQA

### What is `useLayoutEffect` and Why we need it

If your Effect wasn’t caused by an interaction (like a click), React will generally let the browser paint the updated screen first before running your Effect. If your Effect is doing something visual (for example, positioning a tooltip), and the delay is noticeable (for example, it flickers), replace `useEffect` with `useLayoutEffect`.

`useLayoutEffect` is a version of `useEffect` that fires before the browser repaints the screen. But `useLayoutEffect` can hurt performance. Prefer useEffect when possible.

**tooltip** is a good example to tell why we need `useLayoutEffect` in some cases.
If there’s enough space, the tooltip should appear above the element, but if it doesn’t fit, it should appear below. In order to render the tooltip at the right final position, you need to know its height (i.e. whether it fits at the top).
To do this, you need to render in two passes:

1. Render the tooltip anywhere (even with a wrong position).
2. Measure its height and decide where to place the tooltip.
3. Render the tooltip again in the correct place.

All of this needs to happen before the browser repaints the screen.

### When React run `setup` in `useEffect`? the detail!!

### What do "paint the updated screen first before running your Effect" means? Why React run in this order. The code inside the useEffect may update the screen as well. Why not run Effect and updated Screen together.

### The life cycle of function component around useEffect?

