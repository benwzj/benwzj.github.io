---
layout: post
title: React Overview with Function Component
date: 2023-10-12
tags: React Web-page Redux
category: React
---

Now React have ditched Class Component and focus on Function Component and Hooks. 
This Overview focus on Function Component. 

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

## Questions

### What is render, How render works.

When states or props change, React will render the component. How to render? Simply say, React will execute the function again(same with render() in class component). But this process have some rules to follow.
For exemple, 
- The initialer of useState will just run at the first time.
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

### What is `export default`
The `export default` prefix lets you mark the main function in a file so that you can later import it from other files. 