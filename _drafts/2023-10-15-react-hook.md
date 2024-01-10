---
layout: post
title: React Hook
date: 2023-10-12
tags: React Hook
category: React
---

## React State system

Call `useState` at the top level of your component to declare a state variable.

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
You can pass 
1. the next state directly, OR 
2. a 'updater function' that calculates it from the previous state.

```js
const [name, setName] = useState('Edward');
const [age, setAge] = useState(28);

function handleClick() {
  setName('Taylor');
  setAge(a => a + 1);
  // ...
}
```

**setState function Caveats** 

- The set function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call. It is Async.
- If the new value you provide is identical to the current state, as determined by an Object.is comparison, React will skip re-rendering the component and its children. 
- React batches state updates. It updates the screen after all the event handlers have run and have called their set functions. 

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

## Synchronize With External System

Some components need to stay connected to the network, some browser API, or a third-party library, while they are displayed on the page. These systems aren’t controlled by React, so they are called external.

If you’re not connecting to any external system, you probably don’t need an Effect.

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
3. If dependencies is [], it will only run after the initial render. ##  useEffect vs. useLayoutEffect.

If your Effect wasn’t caused by an interaction (like a click), React will generally let the browser paint the updated screen first before running your Effect. If your Effect is doing something visual (for example, positioning a tooltip), and the delay is noticeable (for example, it flickers), replace useEffect with useLayoutEffect.

### `useEffect` Questions

- When React run `setup` in `useEffect`? the detail!!

- What do "paint the updated screen first before running your Effect." means?

- Clear the life cycle of function component around useEffect!
