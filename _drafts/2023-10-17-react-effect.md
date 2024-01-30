---
layout: post
title: Synchronize With External System
date: 2023-10-17
tags: React Hook
category: React
---

Some components need to stay connected to the **network**, some **browser API**, or a **third-party library** while they are displayed on the page. These systems aren’t controlled by React, so they are called external.

Effects let you run some code after rendering so that you can synchronize your component with some system outside of React. 

If you’re not connecting to any external system, you probably don’t need an Effect.

## What are Effects

When talk about Effect, usually compare to Event. 
Effects let you specify side effects that are caused by rendering itself, rather than by a particular event. 

> Capitalized “Effect” refers to the React-specific definition above, i.e. a side effect caused by rendering. To refer to the broader programming concept, we’ll say “side effect”.

To declare an Effect in your component, you need to import the `useEffect` Hook from React.

## The `useEffect`

### Why use `useEffect`
useEffect lets you synchronize a component with an external system.

### What is `useEffect？`
useEffect is a React Hook.

### How do `useEffect` work
```js
useEffect(setup, dependencies?)
```
- setup: The function with your Effect’s logic. Your setup function may also optionally return a cleanup function.
- optional dependencies: The list of all reactive values referenced inside of the setup code. Reactive values include props, state, and all the variables and functions declared directly inside your component body.
1. If you specify the dependencies, your Effect runs after the initial render and after re-renders with changed dependencies.
2. If you omit this argument, your Effect will re-run after every re-render of the component.
3. If dependencies is [], it will only run after the initial render. 

### The Design of useEffect

## Life Cycle of Effect

## FQA

- When React run `setup` in `useEffect`? the detail!!

- What do "paint the updated screen first before running your Effect." means?

- Clear the life cycle of function component around useEffect!

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