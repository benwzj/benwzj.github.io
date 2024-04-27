---
layout: post
title: React Component Detail
date: 2021-07-11
categories: React
tags: Component React
toc: 
  - name: Component Basic
  - name: Class vs. Function Component
  - name: Component Life cycle
  - name: PureComponent vs Component
  - name: Web component vs React component
---

## Component Basic

Components are one of the core concepts of React. 
Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen. React lets you combine your markup, CSS, and JavaScript into custom “components”, reusable UI elements for your app. Just like with HTML tags, you can **compose**, order and nest components to design whole pages. 


### Component Features
- Component is independent and reuseable code.
- Component can be in another Component.  
- Component work as two ways: **class** and **function**. 
- __state__ and __prop__ are key properties for components.
- Compoents' names must start with a capital letter or they won’t work!

### Props 

- __props__ are a way of passing data from parent to child. 
- __props__ contains information set by the parent component (although defaults can be set) and should not be changed.
- Can pass HTML's attributes as a props to React class. And attribute can be a OBJECT.

### State

- All React component have built-in state object. state object is the object in which you can store component properties.
- State is reserved only for interactivity, Static version don't need state.
- State is private and fully controled by component. e.g. initialise, change.
- USE setState to update component state properties. 
- `setState()` can cause re-render.
- `setState()` use asychronous nature. React may batch multiple `setState()` calls into a single update for performance.

### Event 
- React Evert are writen in camelCase syntax:  use `onClick()` instead of `onclick()`;
- Use curly braces: use `onClick={shoot}` instead of `onclick='shoot()'`.
- Always use arrow function, because with arrow function, keyword this will always represent the object which define it. 
		
### passing Arguments to Event handler, TWO WAYS:
- make anonymous arrow function. like:
```js
<button onClick = {()=>this.myFunction('Goal')}> take shot </button>
```
- bind event handler to 'this';like: 
```js
<button onClick = {this.myFunction.bind(this,'Goal')}> take shot </button>
```
- handling form passing event object, you can use `event.target.value` on `onChange ()` or `onClick()`;

example:
inside component:
```js
  myChange = (event)=>{
    this.setState({username : event.target.value});
  }
```
event element:
```js
	onChange = {this.myChange}
```

## Class vs. Function Component

These two kinds of components are equivalent from React’s point of view.
But React recommend defining components as functions instead of classes.
- The function of the Function Component is run through with each render. (But `useRef()` can return the same object for each time render)
- Class component instance keep alive. the `instance.render()` with will run with each render

### Syntax:
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

### prop
Props are passed from outside into the components.
- For Class component, Props are passed as arguments to the constructor and also should be passed to the parent component class by calling `super(props)`. this.props are accessible throughout the life of the object.
- For Function component, props are passed as arguments into the function. 

### state
- For Class component, React.Component objects have property state, so it is stateful.
- For Function components, although they are known as stateless components, but you can use hook `useState()` to manage state. 

### Life cycle
- For Class component, React lifecycle methods can be used inside class components (for example, `componentDidMount`).
- For Function component, you can use hook `useEffect()` to manage lifecycle.

### Life time
- Functional component run from top to bottom and once the function is returned it can’t be kept alive. There are no instance object kept alive. 
- Class component is instantiated and different life cycle method is kept alive and being run and invoked depending on phase of class component.

### Render
- For Function component, It have not instance, When it need to render, the function component gets called which returns a set of instructions for creating DOM.
- For Class component, When it need to render, instance’s render method gets called which returns a set of instructions for creating DOM.

### If you need to use regular function in class component. 
You have to bind `'this'` to component instance. otherwise , this will be undefined.
```js
constructor(props){
	super(props);
	this.myFunction = this.myFunction.bind(this);
}
myFunction(){
	alert (this);
}
```	

## Component Life cycle

There are three phases which are: **Mounting**, **Updating**, and **Unmounting**.

{% include figure.html path="assets/img/component-lifecycle.png" class="img-fluid rounded z-depth-1" %}

### Mounting: four built-in methods that gets called, in this order:

1. `constructor()` optional, handle the props, and pass props to `super(props)`;
2. `static getDerivedStateFromProps()`, rare use
3. `render()`, is required, and is the method that actually outputs the HTML to the DOM.
4. `componentDidMount()`, is called after the component is rendered.

### Updating

A component is updated whenever there is a change in the component's state or props.
Five built-in methods that gets called:
1. `getDerivedStateFromProps()`
2. `shouldComponentUpdate()`
3. `render()`
4. `getSnapshotBeforeUpdate()`
5. `componentDidUpdate()`, Use this as an opportunity to operate on the DOM when the component has been updated.
```js
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```
> You may call `setState()` immediately in `componentDidUpdate()` but note that it must be wrapped in a condition.

### UnMounting

Just this method: `componentWillUnmount()`

### Questions
- It is easier to understand function components, which return react element. But how to understand Class component which use `render()`? 
- Why `render()` function happen both in mounting and updating phrase?

## PureComponent vs Component

React.PureComponent is similar to React.Component. 
The difference between them is that 
- React.PureComponent implements shouldComponentUpdate() with a shallow prop and state comparison.
- React.Component doesn’t implement shouldComponentUpdate() by default. But if you are confident you want to write it by hand, you may compare this.props with nextProps and this.state with nextState and return false to tell React the update can be skipped. 

## Web component vs React component

Web component and React component are in different scope. 
Web Component is a web technology, that means it is a specification of w3c. While React component is a concept in React.

### What is Web Component
Web Components are custom elements that you can define and reuse in your Web apps.
Web Component consists of three main technologies: 
1. Custom elements: APIs to define new HTML elements
2. Shadow DOM: encapsulated DOM and styling, with composition
3. HTML templates: HTML fragments that are not rendered, but stored until instantiated via JavaScript

Web component basic approach: 
1. Using class syntax, create a class. 
2. Register your new custom element using the `CustomElementRegistry.define()` method.
3. attach a shadow DOM to the custom element using `Element.attachShadow()` method.
4. define an HTML template using `<template>` and `<slot>`.
5. Use your custom element wherever you like on your page, just like you would any regular HTML element.

React and Web Components are built to solve different problems. Web Components provide strong encapsulation for reusable components, while React provides a declarative library that keeps the DOM in sync with your data. The two goals are complementary. You are free to use React in your Web Components, or to use Web Components in React, or both.

Most people who use React don’t use Web Components, but you may want to, especially if you are using third-party UI components that are written using Web Components.
