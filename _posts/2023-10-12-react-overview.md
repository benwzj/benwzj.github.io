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

## React Rules

### Component Rules

- Keeping Components Pure
- If you can, update all the relevant state in the event handlers.
- If you want to reset the entire component tree’s state, pass a **different key** to your component.
- Avoid updating state in an effect
- You can store information from previous renders, but need to use condition, and also, the logic is hard to read. try to avoid.
- When you call the `set` function of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the children. 
- State behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render.
- React will ignore your update if the next state is equal to the previous state, as determined by an Object.is comparison. 
- In Strict Mode, React will call some of your functions twice instead of once.


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
You have to put `() =>` before them in both cases. Then React will store the functions you pass.
```js
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

## synchronize with external system

Some components need to stay connected to the network, some browser API, or a third-party library, while they are displayed on the page. These systems aren’t controlled by React, so they are called external.

If you’re not connecting to any external system, you probably don’t need an Effect.

### Why use `useEffect`?
useEffect lets you synchronize a component with an external system.

### What is `useEffect？`
useEffect is a React Hook.

### How do `useEffect` work?
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

- What do render() doing? What do re-render() doing ? what do "paint the updated screen first before running your Effect." means?

- Clear the life cycle of function component around useEffect!


## Component life cycle

### Render vs. Mount vs. Re-render

- "Rendering" is any time a function component gets called (or a class-based render method gets called) which returns a set of instructions for creating DOM.
- "Mounting" is when React "renders" the component for the first time and actually builds the initial DOM from those instructions.
- A "re-render" is when React calls the function component again to get a new set of instructions on an already mounted component.

## React Under Hood

### Renderer
The renderer is the part of the React ecosystem responsible for displaying React components on specific platforms (Web, Mobile, CLI).
There are three officially supported renderers: React Dom, React Native, and React Test.
In addition, there are MANY custom renderers. 

### Reconciliation
React’s diffing algorithm is called reconciliation.

Underneath, all renderers share some common logic. That shared part is encapsulated in the Reconciler. This is the core algorithm that is independent from any platform.
Earlier versions of React were powered by the so-called Stack Reconciler. It has been in use up to version 15 of React. With the release of React 16, we received a greatly improved reconciler algorithm, the Fiber Reconciler.

### keys
Keys is a way which can help React Reconciliation algorithm to solve the inefficiency issue. Especially in list items.

Keys should be stable, predictable, and unique. Unstable keys (like those produced by Math.random()) will cause many component instances and DOM nodes to be unnecessarily recreated, which can cause performance degradation and lost state in child components.

Finding a key for component:
- The element you are going to display may already have a unique ID.
- you can add a new ID property to your model or hash some parts of the content to generate a key. The key only has to be unique among its siblings, not globally unique.
- As a last resort, you can pass an item’s index in the array as a key. This can work well if the items are never reordered, but reorders will be slow.

### refs
Refs provide a way to access DOM nodes or React elements created in the render method.

## Questions

### What is render(), How render works.

when states or props change, React will render the component. How to render? Simply say, React will execute the function again. But this process have some rules to follow.
For exemple, 
- the initialer of useState will just run at the first time.
- When you call the `set` function of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the childre.

### How to Upward Communication
To communicate from a child to a parent component we can pass a function handler from parent to child.

Take a look at this child component.  Whenever the user clicks on the button, it tries to call a function passed down to it through the props system.
```js
function Child({ onFunctionFromParent }) {
  const handleClick = () => {
    onFunctionFromParent('i am very good!');
  }
 
  return <button onClick={handleClick}>Click Here!</button>
}
```
To use this component and be told when a user clicks the button
```js
function Parent() {
  const handleMessageFromChild = (message) => {
    console.log("message from child is "+ message)
  }
 
  return <Child onFunctionFromParent={handleMessageFromChild} />
}
```

### What is Root component
The React application begins at a “root” component. Usually, it is created automatically when you start a new project. For example, if you use CodeSandbox or if you use the framework Next.js, the root component is defined in pages/index.js.

### What is Defining a component
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
React don't recommend to use Class components for new code.

