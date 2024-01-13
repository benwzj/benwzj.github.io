---
layout: post
title: React State system
date: 2023-10-15
tags: React Hook
category: React
---

## State Rules
- Treat all state in React as immutable. This help React run very fast. using Immer for object state is good choice.
- React batches state updates, it will queue all set functions and execute all set functions one by one before re-render.
- State behaves as a snapshot. Setting state does not change the state variable you already have, but instead triggers a re-render.

## `useState` Hook

```js
const [state, setState] = useState(initialState);
```
useState returns an array with exactly two items:
1. The current state of this state variable, initially set to the initial state you provided.
2. The set function that lets you change it to any other value in response to interaction.

Important to Know:
1. Calling the set function does not change the current state in the already executing code.
2. State is considered read-only, When state is objects or arrays, you should replace it rather than mutate your existing objects.
3. About the initial state, React saves it once and ignores it on the next renders. So don't do this: `const [todos, setTodos] = useState(createInitialTodos());`, because React run this function every render and means nothing. But you can do this: `const [todos, setTodos] = useState(createInitialTodos);`

### `setState` function

The set function returned by `useState` lets you update the state to a different value and trigger a re-render. 

You can pass the next **state value** directly, OR a **'updater function'** that calculates it from the previous state. 
Because **React batches state updates**, it will queue all these set functions and execute all these set functions one by one before re-render. React will treat updater function differently from direct value (`baseState` is current value; `queue` is setfunctions array):
```js
export function getFinalState(baseState, queue) {
  let finalState = baseState;

  for (let updater of queue) {
    if (typeof updater === 'function') {
      // Apply the updater function.
      finalState = updater(finalState);
    } else {
      // Replace the next state.
      finalState = updater;
    }
  }

  return finalState;
}
```

- The set function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call. It is Async.
- If the new value you provide is identical to the current state, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. 
- Calling the set function during rendering is only allowed from within the currently rendering component. React will discard its output and immediately attempt to render it again with the new state. This pattern is rarely needed, but you can use it to store information from the previous renders.
- In Strict Mode, React will call your updater function twice in order to help you find accidental impurities. This is development-only behavior and does not affect production.
- Don't do this:

```js
const [fn, setFn] = useState(someFunction);

function handleClick() {
  setFn(someOtherFunction);
}
```
You have to put `() =>` before them in both cases. Then React will store the functions you pass.
```js
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

