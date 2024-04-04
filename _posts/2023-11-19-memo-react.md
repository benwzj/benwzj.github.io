---
layout: post
title: Use Memory in React
date: 2023-11-19
category: React
tags: React Hook
toc: 
  - name: API `memo`  
  - name: Hook `useMemo`
  - name: Hook `useCallback`
---

## API `memo`

React normally re-renders a component whenever its parent re-renders.
With `memo`, you can create a component that React will not re-render when its parent re-renders so long as its new props are the same as the old props. Such a component is said to be memoized.

### How to check if a compnent is re-rendered

Re-render a component, means React execute that component function to calculate the output.

```js
let count = 0;
const Component = () => {
count++;
console.log("component render number: ", count);
//...
}
```

But the component might be only invoked without rerendering. 
For example, you `setState()` to the same value as previous one, React will run the component function, but without rendering the children or firing effects.

So the better way is to make a `useEffect` hook without a dependency array, this will make it run after each component render.

How to display render count in react component?
You can't use State, but you can use **Ref**!!
```js
import { useRef, useEffect } from "react";
export const Counter = props => {
    const renderCounter  = useRef(0);
    useEffect (()=>{
      renderCounter.current = renderCounter.current + 1;
    });
    return <h1>Renders: {renderCounter.current}, {props.message}</h1>;
};
```

### How to Use 

`const MemoizedComponent = memo(SomeComponent, arePropsEqual?)`

Wrap a component in `memo` to get a memoized version of that component. This memoized version of your component will usually not be re-rendered when its parent component is re-rendered as long as its props have not changed. But React may still re-render it: memoization is a performance optimization, not a guarantee.

To memoize a component, wrap it in `memo` and use the value that it returns in place of your original component:
```js
import { memo } from 'react';

const SomeComponent = memo(function SomeComponent(props) {
  // ...
});
```

### Usage 

- Skipping re-rendering when props are unchanged
- Updating a memoized component using state
- Updating a memoized component using a context
- Minimizing props changes
- Specifying a custom comparison function


## Hook `useMemo`

`useMemo` is a React Hook that lets you cache the result of a calculation between re-renders. You should only rely on `useMemo` as a **performance optimization**. 
Caching return values like this is known as **memoization**, which is why this Hook is called `useMemo`.

> Main purpose of `useMemo` is Skipping expensive recalculations.

### Optimizing with useMemo is only valuable in a few cases:

- The calculation you’re putting in useMemo is noticeably slow, and its dependencies rarely change.
- You pass it as a prop to a component wrapped in [memo](https://react.dev/reference/react/memo) API. You want to skip re-rendering if the value hasn’t changed. Memoization lets your component re-render only when dependencies aren’t the same.
- The value you’re passing is later used as a dependency of some Hook. For example, maybe another useMemo calculation value depends on it. Or maybe you are depending on this value from useEffect.

### How to tell if a calculation is expensive? 

You can add a console log to measure the time spent in a piece of code:
```js
console.time('filter array');
const visibleTodos = getFilteredTodos(todos, filter);
console.timeEnd('filter array');
```
If the overall logged time adds up to a significant amount (say, 1ms or more),  it might make sense to memoize that calculation. 

### Using useMemo

Syntax:
`const cachedValue = useMemo(calculateValue, dependencies)`
Reture:
- On the initial render, useMemo returns the result of calling calculateValue with no arguments.
- During next renders, it will either return an already stored value from the last render (if the dependencies haven’t changed), or call calculateValue again, and return the result that calculateValue has returned.

Example:
```js
import { useMemo } from 'react';

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab]
  );
  // ...
}
```

## Hook `useCallback`

Returns a memoized callback.

`useCallback()` often is used in conjunction with `useEffect()` because it allows you to prevent the re-creation of a function. For this, it's important to understand that functions are just objects in JavaScript.

In a functional component, any function you define inside of it is re-created whenever the component rebuilds. Normally it is no problem, that innerFunction re-created for every render cycle. But it become a problem if the innerFunction become a dependency of `useEffect()`. If innerFunction cause component rebuild, then it will infinite loop.  
Use `useCallback()` can prevent that happen.

- `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`.
- `useCallback` will return a memoized version of the callback that only changes if one of the dependencies has changed. 
- This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders .


