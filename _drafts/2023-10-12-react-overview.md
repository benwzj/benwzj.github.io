---
layout: post
title: React Overview with Hook
date: 2023-10-12
tags: React Web-page Redux
category: React
---

I have writen a React Overview years ago, but that was based on Class Component. Now React have ditched Class Component and focus on Function Component and Hooks. 

This Overview will follow new trend of React. 

Components are one of the core concepts of React.
Components are the foundation upon which you build user interfaces (UI).

The `export default` prefix lets you mark the main function in a file so that you can later import it from other files. 

## React frameworks
If you want to build a new app or a new website fully with React, use frameworks. 

### Why Frameworks
If you just want to run React, then what you need just grab react and react-dom from npm. 
But as a Web App, it still need routing, data fetching, and generating HTML for good performance.
Frameworks provide features that most apps and sites eventually need, including routing, data fetching, and generating HTML.

- **Next.js** is a full-stack React framework
- **Remix** is a full-stack React framework with nested routing
- **Gatsby** is a React framework for fast CMS-backed websites
- **Expo** (for native apps) is a React framework that lets you create universal Android, iOS, and web apps with truly native UIs. 

## root component
The React application begins at a “root” component. Usually, it is created automatically when you start a new project. For example, if you use CodeSandbox or if you use the framework Next.js, the root component is defined in pages/index.js.

## Defining a component
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
React don't recommend to use Class components for new code.

## React State system

Call `useState` at the top level of your component to declare a state variable.

```js
const [state, setState] = useState(initialState);
```
useState returns an array with exactly two items:
1. The current state of this state variable, initially set to the initial state you provided.
2. The set function that lets you change it to any other value in response to interaction.

> ##### **Important**
> 
> 1. Calling the set function does not change the current state in the already executing code.
> 2. State is considered read-only, When state is objects or arrays, you should **replace** it rather than mutate your existing objects.
> 3. About the initial state, React saves it once and ignores it on the next renders. So don't do this: `const [todos, setTodos] = useState(createInitialTodos());`, because React run this function every **render** and means nothing. But you can do this: `const [todos, setTodos] = useState(createInitialTodos);`
{: .block-warning }

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
Uou have to put `() =>` before them in both cases. Then React will store the functions you pass.
```js
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

## React Rules

### Component Ruels

- Keeping Components Pure
- If you can, update all the relevant state in the event handlers.
- If you want to reset the entire component tree’s state, pass a **different key** to your component.
- Avoid updating state in an effect
- You can store information from previous renders, but need to use condition, and also, the logic is hard to read. try to avoid.
- When you call the `set` function of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the children. 
- State behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render.
- React will ignore your update if the next state is equal to the previous state, as determined by an Object.is comparison. 
- In Strict Mode, React will call some of your functions twice instead of once.


## Questions

- What is render(), How render works.
- When will execute Function Component's function.
- What is life cycle of function component.


