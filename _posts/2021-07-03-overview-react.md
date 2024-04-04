---
layout: post
title: My Overview of React
date: 2021-07-03
categories: React
tags: Hook Redux Overview React
---

## What is React

React is a JavaScript library. It provide a framework for you to build your interactive UIs. It is a declarative library. It lets you compose complex UIs from small and isolated pieces of code called “components”.

### Here are some key features of React:
- React is using component composition for code reuse.
- React provides a declarative API so that you don’t have to worry about exactly what changes on every UI update. This makes writing applications a lot easier.
- React create a virtual DOM in memory. 
- React only change what need to be changed. 
- React can work without node.js, just need some tools. like npm, Babel, Webpack
- React’s one-way data flow (also called one-way binding) keeps everything modular and fast.
- Using **JSX**, which can be translated by Babel.
- There are two types of “model” data in React: props and state.  
- In React components, code reuse is primarily achieved through composition rather than inheritance.

### The simplest React example:
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
} // component

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```

### React library
All top-level APIs come from 'react' library, including hooks.
So, you need to always import react!
`import React, { useState, useEffect } from 'react';`

### ReactDOM package
The react-dom package provides DOM-specific methods that can be used at the top level of your app and as an escape hatch to get outside the React model if you need to.

React dom is one of the renderers.
The renderer is the part of the React ecosystem responsible for displaying React components on specific platforms. like Web dom, mobile native. 

Usually we use it at index.js as below:
```javascript
import ReactDOM from 'react-dom';
ReactDOM.render(<App />, document.querySelector('#root'));
```

### Ajax in React
You can use any AJAX library you like with React. Like Axios, jQuery AJAX, and the browser built-in `window.fetch`.

You should populate data with AJAX calls in the `componentDidMount` lifecycle method. This is so you can use `setState` to update your component when the data is retrieved.

## Concepts

Component is core concept in React. We also need to clear what is React Instances, React Element.

### Components Encapsulate Element Trees.
For a React component, props are the input, and an element tree is the output.

The returned element tree can contain both elements describing DOM nodes, and elements describing other components. This lets you compose independent parts of UI without relying on their internal DOM structure.

whether functions or classes, fundamentally they are all components to React. They take the props as their input, and return the elements as their output.

We let React create, update, and destroy instances. We describe them with elements we return from the components, and React takes care of managing the instances.

### What is React Instances

We let React create, update, and destroy instances. We describe them with elements we return from the components, and React takes care of managing the instances.

Only components declared as classes have instances, and you never create them directly: React does that for you. The Only reason to use this instance is for imperative actions (such as setting focus on a field), and should generally be avoided.

Instances have much less importance in React than in most object-oriented UI frameworks.

### What is React Element

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

Simple element example: 

DOM tags:
```javascript
const element = <h1>Hello, world</h1>;
```
user-defined components.
```javascript
const element = <Welcome name='sara' />;
```

React element structure 
```javascript
const App = () => {
  return <p>Hello React</p>;
};
```
React calls its `React.createElement()` method internally which returns the following object:
```javascript
console.log(App());
// {
//   $$typeof: Symbol(react.element)
//   "type": "p",
//   "key": null,
//   "ref": null,
//   "props": {
//     "children": "Hello React"
//   },
//   "_owner": null,
//   "_store": {}
// }
```

### React Elements Describe the Tree
In React, An element contains only information about the component type (for example, a Button), its properties (for example, its color), and any child elements inside it.

An element is not an actual instance. Rather, it is a way to tell React what you want to see on the screen. It’s just an immutable description object with fields like: 
type: (string | ReactClass) and 
props: Object.

1. DOM Elements: type is string, it represents a DOM node with that tag name, and props correspond to its attributes. They don’t refer to anything on the screen when you create them. 
React elements are easy to traverse, don’t need to be parsed, and of course they are much lighter than the actual DOM elements—they’re just objects!
2. Component Elements: the type of an element can also be a function or a class corresponding to a React component. 

An element describing a component is also an element, just like an element describing the DOM node. They can be nested and mixed with each other.

### React component vs. React element vs. component instance

- Rendering a component happens whenever we use this component as a React element with angle brackets (e.g.` <Greeting />`) in another component:
`const this_is_element = <ThisIsComponent />`
- Whenever a component gets rendered as element, we create an instance of this component.
- While a React Component is the one time declaration of a component, it can be used once or multiple times as React Element in JSX (React's createElement method).

### HTML element vs. React element

HTML elements (or call DOM elements) are created by tag, like` <div> <a>` etc.
React elements are plain objects.

### Web component vs. React component

- Web component and React component are in different scope. Web Component is a web technology, that means it is a specification of w3c. While React component is a concept in React.
- Web Components are custom elements that you can define and reuse in your Web apps.
- React and Web Components are built to solve different problems. Web Components provide strong encapsulation for reusable components, while React provides a declarative library that keeps the DOM in sync with your data. The two goals are complementary. You can use React in your Web Components, or to use Web Components in React.

## Component Detail

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. 

### Props 

- props are a way of passing data from parent to child. 
- props contains information set by the parent component (although defaults can be set) and should not be changed.

### State

- State is reserved only for interactivity, Static version don't need state.
- State is private and fully controled by component. e.g. initialise, change.
- setState() can cause re-render.
- setState() use asychronous nature. React may batch multiple setState() calls into a single update for performance.

### Class vs. Function Component

These two kinds of components are equivalent from React’s point of view.

- The function of the Function Component is run through with each render. (But useRef() can return the same object for each time render)
- Class component instance keep alive. the instance.render() with will run with each render

Syntax:
- A functional component is just a plain JavaScript pure function that accepts props as an argument and returns a React element(JSX).
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

- A class component requires you to extend from React.Component and create a render function which returns a React element.
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

**Props** are passed from outside into the components:
- For Class component, Props are passed as arguments to the constructor and also should be passed to the parent component class by calling super(props). this.props are accessible throughout the life of the object.
- For Function component, props are passed as arguments into the function. 

Handing **state**:
- For Class component, React.Component objects have property state, so it is stateful.
- For Function components, although they are known as stateless components, but you can use hook useState() to manage state. 

Life cycle
- For Class component, React lifecycle methods can be used inside class components (for example, componentDidMount).
- For Function component, you can use hook useEffect() to manage lifecycle.

Life time
- Functional component run from top to bottom and once the function is returned it can’t be kept alive. There are no instance object kept alive. 
- Class component is instantiated and different life cycle method is kept alive and being run and invoked depending on phase of class component.

Render
- For Function component, It have not instance, When it need to render, the function component gets called which returns a set of instructions for creating DOM.
- For Class component, When it need to render, instance’s render method gets called which returns a set of instructions for creating DOM.

### Component Life cycle

There are three phases which are: Mounting, Updating, and Unmounting.
{% include figure.html path="assets/img/ComponentLiftCycle.png" class="img-fluid rounded z-depth-1" %}

#### Mounting: four built-in methods that gets called, in this order:

1. `constructor()` optional, handle the props, and pass props to super(props);
2. static `getDerivedStateFromProps()`, rare use
3. **`render()`**, is required, and is the method that actually outputs the HTML to the DOM.
4. **`componentDidMount()`**, is called after the component is rendered.

#### Updating

A component is updated whenever there is a change in the component's state or props.
five built-in methods that gets called
1. `getDerivedStateFromProps()`
2. `shouldComponentUpdate()`
3. **`render()`**
4. `getSnapshotBeforeUpdate()`
5. **`componentDidUpdate()`**, Use this as an opportunity to operate on the DOM when the component has been updated.
```js
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```
You may call `setState()` immediately in `componentDidUpdate() `but note that it must be wrapped in a condition.

#### UnMounting

Just use this method:
`componentWillUnmount()`

## Hook

Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

Hooks are functions that let you “hook into” React state and lifecycle features from function components. 

- `useState()` is a Hook. We call it inside a function component to add some local state to it.

- `useEffect()` adds the ability to perform side effects from a function component. It serves the same purpose as componentDidMount, componentDidUpdate, and componentWillUnmount in React classes

Rules of Using Hook:
1. Only Call Hooks at the Top Level
2. Only Call Hooks from React Functions

## Redux

First of All, Redux give you a pattern which can help you manage your application state. And also give you a tools library to help you to build this pattern. 

The main concept of implements this is using events called "actions". 
Redux serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion.

{% include figure.html path="assets/img/redux-overview.png" class="img-fluid rounded z-depth-1" %}

> Redux is not just for react, it can be used to other JS application.


## How to use React

Runing React application, means using React APIs which locate in React library. React is the entry point to the React library. If you don’t use bundler and you just load React from a `<script>` tag, these top-level APIs are available on the React global. 
- If you use ES6 with npm, you can write `import React from 'react'`. 
- If you use ES5 with npm, you can write `var React = require('react')`.

### How do react applications work within browsers

React app usually have one index.html and many js files. React use Webpack tool to bundle all the js files into one bundle.js file. 

When browsers visit React app, it will return index.html file. And also, return the bundle js file! Browser will execute the bundle file!

### How do react work with the backend
React is front end application framework. Just like HTML file, it can connect to backend through Hyperlinks, Ajax, etc. 

### How to deploy react

- Create React App
Using Create React App to create React Application. and you can use it with any backend you want. 
Under the hood, Create React App uses Babel and webpack, but you don’t need to know anything about them.
When you’re ready to deploy to production, running npm run build will create an optimized build of your app in the build folder. 

- Vercel
There are many services which provide web application deployment. Like Vercel. 
Vercel is free and easy to use. Sign in and Run CLI to deploy your code to Vercel. after done, you can get a public link like: https//yourapplicationname.vercel.app
Under the hood, Vercel use `npm run build` to build your app.

- Next.js framework
You can use Next.js to build Node.js applications built with React. 
It includes styling and routing solutions out of the box.

## How to design React applications?

How do you know what should be its own component? 
One such technique is the single responsibility principle, that is, a component should ideally only do one thing. 

arrange components into a hierarchy.

Usually it can be static version and interactive version. 
- It’s best to decouple these processes 
because building a static version requires a lot of typing and no thinking, and adding interactivity requires a lot of thinking and not a lot of typing.  
- To build a static version of your app that renders your data model, you’ll want to build components that reuse other components and pass data using props. props are a way of passing data from parent to child. 
- To make your UI interactive, you need to be able to trigger changes to your underlying data model. React achieves this with state. go through each one and figure out which one is state.
