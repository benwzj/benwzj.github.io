---
layout: post
title: React Overview with Function Component
date: 2023-10-12
tags: React Web-page Redux
category: React
toc: 
  - name: React Rules
  - name: Component life cycle
  - name: React Under Hood
  - name: React frameworks
  - name: Hooks
  - name: What Rendering means in React
  - name: Inverse Data Flow
  - name: FQA
---

Now React have ditched Class Component and focus on Function Component and Hooks. 
This Overview focus on Function Component. 

React allows you to write maintainable and performant code by using concept of component. Components allow you to focus on describing the UI you want, rather than focusing on the details of how the UI actually gets inserted in the page.

## React Rules

### Component Rules

- Keeping Components Pure.
- Only call Hooks at the top level.
- If you can, update all the relevant state in the event handlers.
- If you want to reset the entire component tree’s state, pass a **different key** `prop` to your component.
- In Strict Mode, React will call some of your functions twice instead of once.

### Event Rules
- All events **propagate** (or calling bubbles) in React except `onScroll`, which only works on the JSX tag you attach it to. But you can prevent an event from reaching parent component by calling `e.stopPropagation()`. On the other hand, `onClickCapture` is special event, React will travels down, calling all onClickCapture handlers.
- Some browser events have default behavior associated with them. For example, a `<form>` submit event, which happens when a button inside of it is clicked, will reload the whole page by default. Calling `e.preventDefault()` to prenvent this happen.

## Component life cycle
Every React component goes through the same lifecycle:

- A component **mounts** when it’s added to the screen.
- A component **updates** when it receives new props or state, usually in response to an interaction.
- A component **unmounts** when it’s removed from the screen.

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


## React frameworks

If you want to build a new app or a new website fully with React, React reommend to use frameworks. Like NextJS, Remix, etc. 

### Why Frameworks
If you just want to run React, then what you need just grab `react` and `react-dom` from npm. 
But as a Web App, it still need **routing**, **data fetching**, and **generating HTML** for good performance.
Frameworks provide features that most apps and sites eventually need, including routing, data fetching, and generating HTML.

- **Create React App (CRA)** ,is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration. Usually, we use CRA to start React app. But React reommend to use framework, like NextJS, to build app. And React work as dependence.
- **NextJS** is a full-stack React framework, provides server-side rendering (SSR).
- **Remix** is a full-stack React framework with nested routing.
- **Gatsby** is a React framework for fast CMS-backed websites.
- **Expo** (for native apps) is a React framework that lets you create universal Android, iOS, and web apps with truly native UIs. 

## Hooks

**eslint-plugin-react-hooks**, This ESLint plugin enforces the Rules of Hooks.

### What is hooks

- Hooks are functions that let you ‘hook into’ React state and lifecycle features from function components.
- They let you use state and other React features without writing a class.	
- React provides a few built-in Hooks like useState. You can also create your own Hooks to reuse stateful behavior between different components. 
- Hooks are JavaScript functions, but they impose two additional rules:
  - Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions.
  - Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions.
- We have two basic Hooks: `useState`, `useEffect`.
- Other Hooks like: `useMemo`, `useCallback`, `useContext`, `useRef`, `useReducer`, etc.
	
### Why hooks

When the components are geting bigger and bigger. there are lots of wrapper, from class components. And stateful logic and side effects get MESS!
- Facebook React team think : 
  1. React needs a better primitive for sharing stateful logic. 
  2. They even think classes can be a large barrier to learning React. 
  3. the lifesycle methods make componet hard to split to smaller components. 
  4. some case it is impossible to break the component into small components because the stateful logic is all over the place. 

- To solve these problems, Hooks let you use more of React’s features without classes.
- use hooks is easy to reuse stateful logic. it is much easier then class components.  	
				
### NO.1 useState ()
just like class conponent state! 
We call it inside a function component to add some local state to it. React will preserve this state between re-renders. 
	
### NO.2 useEffect()

When you call useEffect, you are telling React to run your ‘effect’ function after flushing changes to the DOM. 
The second argument of `useEffect()` will decide whether the first argument ( which is a function ) is being called or not. 

### useCallback()

Returns a memoized callback.

`useCallback()` often is used in conjunction with `useEffect()` because it allows you to prevent the re-creation of a function. For this, it's important to understand that functions are just objects in JavaScript.

In a functional component, any function you define inside of it is re-created whenever the component rebuilds. Normally it is no problem, that innerFunction re-created for every render cycle. But it become a problem if the innerFunction become a dependency of `useEffect()`. If innerFunction cause component rebuild, then it will infinite loop.  
Use `useCallback()` can prevent that happen.

- `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`.
- `useCallback` will return a memoized version of the callback that only changes if one of the dependencies has changed. 
- This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders .

### useReducer()

An alternative to `useState()`.
UseReducer is usually preferable to useState when you have complex state logic that involves multiple sub-values or when **the next state depends on the previous one**.

`const [state, dispatch] = useReducer(reducer, initialArg, init);`

- state: it is you data.
- Dispatch: like Redux‘s `dispatch ()`, dispatch action to `reducer()` function. 
- Reducer: it is a function to handle state data according to action. 
- InitialArg: initial state. This way is slightly different from Redux. It is specified by hook call. 
- init: optional function argument, to initialise initial state. 

React guarantees that dispatch function identity is stable and won’t change on re-renders. This is why it’s safe to omit from the `useEffect` or `useCallback` dependency list.

## What Rendering means in React

Before your components are displayed on screen, they must be rendered by React. 

There are 3 steps for the whole rendering process: 
1. Triggering a render.
2. **Rendering the component.**
3. Committing to the DOM.

#### Step 1: Trigger a render 

There are two reasons for a component to render:
- It’s the component’s initial render. By calling `createRoot` with the target DOM node, and then calling its render method with the component.
- The component’s (or one of its ancestors’) state has been updated. Updating the component’s state automatically queues a render. When prop or context be changed, it cause re-render as well.

#### Step 2: React renders your components

> 'Rendering' is React calling your components.
{: .block-warning }

- On initial render, React will call the root component.
- For subsequent renders, React will call the function component whose state update triggered the render.

> The default behavior of rendering will render all components nested within the updated component. 
{: .block-warning }

This might be not optimal for performance if the updated component is very high in the tree. 

#### Step 3: React commits changes to the DOM 
After rendering (calling) your components, React will modify the DOM.
- For the initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen.
- For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.

React only changes the DOM nodes if there’s a difference between renders.

After rendering is done and React updated the DOM, the browser will repaint the screen. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion with React rendering.


## Inverse Data Flow

This is a very common thing you will do in your React App. When you design your app structure, you may prefer to put your `state` in parent component, and it's children component can easily get `state`. And if the children component change the `state`, you just simply pass the `setState()` handler to children. The children then use `setState())` handler to change `state`. This is called Iverse Data Flow.

> ##### The Definition: 
> To communicate from a child to a parent component we can pass a function handler from parent to child.
{: .block-warning}

Let's take a look the example.
Whenever the user clicks on the button, it tries to call a function passed down to it through the props system.
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

## FQA

### What is Root component
The React application begins at a “root” component. Usually, it is created automatically when you start a new project. For example, if you use CodeSandbox or if you use the framework Next.js, the root component is defined in pages/index.js.

### What is Defining a component
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
React don't recommend to use Class components for new code.

### What is `export default`
The `export default` prefix lets you mark the main function in a file so that you can later import it from other files. 

### React is declarative UI programming

In imperative programming, the above corresponds directly to how you implement interaction. You have to write the exact instructions to manipulate the UI depending on what just happened.

In React, declarative UI programming, you don’t directly manipulate the UI—meaning you don’t enable, disable, show, or hide components directly. Instead, you declare what you want to show, and React figures out how to update the UI. 

`state` is the key for React to implement declarative programming.
1. Identify your component’s different visual states
2. Determine what triggers those state changes
3. Represent the state in memory using useState
4. Remove any non-essential state variables
5. Connect the event handlers to set the state

### Strict Mode

React Strict Mode lets you find common bugs in your components early during development.

```js
<StrictMode>
  <App />
</StrictMode>
```

If you are using `Next.js` or `create-react-app` to create your React app. Strict Mode is enabled by default.

Strict Mode enables the following checks in development:
- Your components will re-render an extra time to find bugs caused by impure rendering.
- Your components will re-run Effects an extra time to find bugs caused by missing Effect cleanup.
- Your components will be checked for usage of deprecated APIs.
All of these checks are development-only and do not impact the production build.

You can turn off Strict Mode to opt out of the development behavior.

