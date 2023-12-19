---
layout: post
title: "Talk about Context in React"
date: 2022-05-05
categories: React
tags: Context Redux React
---

## What is Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

In a typical React application, data is passed top-down (parent to child) via props, but such usage can be cumbersome for certain types of props (e.g. locale preference, UI theme) that are required by many components within an application. 

## Use Context

Usually you create, provide, and consume context, here it is:

#### Firstly, Create Context.js file

You'll usually have one file that uses createContext and exports a Provider wrapper:

```javascript
import React, { createContext } from 'react'
export const Context = createContext()
export const Provider = ({ children }) => {
  const [state, setState] = useState({})

  const value = {
    state,
    setState,
  }

  return <Context.Provider value={value}>{children}</Context.Provider>
}
```
It will export `Context` and `Provider`.

#### Secondly, Wrap Consuming Components in App.

You'll wrap whatever component needs access to the Context state with the Provider:

```javascript
import React from 'react'
import { Provider } from './Context'
import { ConsumingComponent } from './ConsumingComponent'

export const Page = () => {
  return (
    <div>
      <Provider>
        <ConsumingComponent />
      </Provider>
    </div>
  )
}
```

#### Thirdly, Build Consuming Component

This is a way to build your Consuming Component: Using the `useContext` hook to access the data:

```js
import React, { useContext } from 'react'
import { Context } from './Context'

export const ConsumingComponent = () => {
  const { state } = useContext(Context)

  return null
}
```

`useContext(MyContext)` is equivalent to `static contextType = MyContext` in a class, or to `<MyContext.Consumer>`.

## Replace Redux 

It is not recommend, but you can do something like Redux: 
1. you can create a react component as a store. and use this component to wrap all children component which need to share information inside them. 
2. use myContext.Provider in the Store component to wrap children components. 
3. design state as object which contain functions. Then children can use function reference to change states.

In a very small application, you might be able to get away with just using Context for most of your global data storage needs, but in a large-scale production environment, you're likely using Redux for global state management. Redux still provides improved performance, improved debugging capabilities, architectural consistency, the ability to use middleware. Therefore, Context is not a replacement for a proper global state management system.


## Important Caveat

Context uses reference identity to determine when to re-render, there are some gotchas that could trigger unintentional renders in consumers when a provider’s parent re-renders. 
For example, the code below will re-render all consumers every time the Provider re-renders because a new object is always created for value.

Class component example:
```js
class App extends React.Component {
  render() {
    return (
      <MyContext.Provider value={{something: 'something'}}>
        <Toolbar />
      </MyContext.Provider>
    );
  }
}
```
How to fix it? Using global reference, like state. 

## Think more before apply context

We should apply context sparingly because it make components reuse more difficult. 
If you only want to avoid passing some props through many levels, **component composition** is often a simpler solution than context. And even **render props** is another option.

> ##### Inversion of Control
>  
> Using component composition, you can wrap the deepest component as prop, then other components don’t need to know what is passing down. This inversion of control can make your code cleaner.
{: .block-warning }

