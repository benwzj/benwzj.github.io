---
layout: post
title: Display Modal in React
date: 2023-11-19-Hooks-React.md
category: React
tags: React Hook
---

## useMemo

`useMemo` is a React Hook that lets you cache the result of a calculation between re-renders. 
Caching return values like this is known as **memoization**, which is why this Hook is called `useMemo`.

Using `useMemo` can Skipping expensive recalculations.
You should only rely on `useMemo` as a performance optimization. 

### How to tell if a calculation is expensive? 

You can add a console log to measure the time spent in a piece of code:
```js
console.time('filter array');
const visibleTodos = getFilteredTodos(todos, filter);
console.timeEnd('filter array');
```

If the overall logged time adds up to a significant amount (say, 1ms or more),  it might make sense to memoize that calculation. 

