---
layout: post
title: Use Memory in React
date: 2023-11-19
category: React
tags: React Hook
toc: 
  - name: Hook `useMemo`
  - name: API `memo`
---

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

## API `memo`

React normally re-renders a component whenever its parent re-renders.
With `memo`, you can create a component that React will not re-render when its parent re-renders so long as its new props are the same as the old props. Such a component is said to be memoized.

> `memo` lets you skip re-rendering a component when its props are unchanged.

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