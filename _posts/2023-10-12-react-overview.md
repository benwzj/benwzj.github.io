---
layout: post
title: React Overview with Function Component
date: 2023-10-12
tags: React Web-page Redux
category: React
---

Now React have ditched Class Component and focus on Function Component and Hooks. 
This Overview focus on Function Component. 

React allows you to write maintainable and performant code by using concept of component. Components allow you to focus on describing the UI you want, rather than focusing on the details of how the UI actually gets inserted in the page.

## React Rules

### Component Rules

- Keeping Components Pure.
- Only call Hooks at the top level.
- If you can, update all the relevant state in the event handlers.
- If you want to reset the entire component tree’s state, pass a **different key** to your component.
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
If you want to build a new app or a new website fully with React, use frameworks. 

### Why Frameworks
If you just want to run React, then what you need just grab react and react-dom from npm. 
But as a Web App, it still need routing, data fetching, and generating HTML for good performance.
Frameworks provide features that most apps and sites eventually need, including routing, data fetching, and generating HTML.

- **Next.js** is a full-stack React framework
- **Remix** is a full-stack React framework with nested routing
- **Gatsby** is a React framework for fast CMS-backed websites
- **Expo** (for native apps) is a React framework that lets you create universal Android, iOS, and web apps with truly native UIs. 

## Hooks

**eslint-plugin-react-hooks**, This ESLint plugin enforces the Rules of Hooks.

## FQA

## What Rendering means in React

Before your components are displayed on screen, they must be rendered by React. 

There are 3 steps for the whole rendering process: 
1. Triggering a render.
2. **Rendering the component.**
3. Committing to the DOM.

#### Step 1: Trigger a render 
There are two reasons for a component to render:
- It’s the component’s initial render. By calling `createRoot` with the target DOM node, and then calling its render method with the component.
- The component’s (or one of its ancestors’) state has been updated. Updating the component’s state automatically queues a render.

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

After rendering is done and React updated the DOM, the browser will repaint the screen. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion throughout the docs.

### There are some rules to follow for this process.
For exemple, 
- The initialer of useState will just run at the first time.
- When you call the `set` function of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the childre.

## How to Upward Communication
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

## What is Root component
The React application begins at a “root” component. Usually, it is created automatically when you start a new project. For example, if you use CodeSandbox or if you use the framework Next.js, the root component is defined in pages/index.js.

## What is Defining a component
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
React don't recommend to use Class components for new code.

## What is `export default`
The `export default` prefix lets you mark the main function in a file so that you can later import it from other files. 