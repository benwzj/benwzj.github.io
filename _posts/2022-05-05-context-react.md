---
layout: post
title: "All About Context in React"
date: 2022-05-05
categories: React
tags: Context Redux React
---

Usually, you will pass information from a parent component to a child component via props. And if you can use props to solve the problems, use props. **BUT** using Context is necessary in some cases. 

For example: 
- Theming: If your app lets the user change its appearance (e.g. dark mode), you can put a context provider at the top of your app, and use that context in components that need to adjust their visual look.
- Current account: Many components might need to know the currently logged in user. Putting it in context makes it convenient to read it anywhere in the tree. Some apps also let you operate multiple accounts at the same time (e.g. to leave a comment as a different user). In those cases, it can be convenient to wrap a part of the UI into a nested provider with a different current account value.
- Routing: Most routing solutions use context internally to hold the current route. This is how every link “knows” whether it’s active or not. If you build your own router, you might want to do it too.
- Managing state: As your app grows, you might end up with a lot of state closer to the top of your app. Many distant components below may want to change it. It is common to use a reducer together with context to manage complex state and pass it down to distant components without too much hassle.

## What is Context

Context is an alternative to passing props. Context lets the parent component make some information available to any component in the tree below it—no matter how deep—without passing it explicitly through props.

Context is not limited to static values. If you pass a different value on the next render, React will update all the components reading it below! This is why context is often used in combination with state.

### Let's sum up first

- Context lets a component provide some information to the entire tree below it.
- To pass context:
  - Create and export it with `export const MyContext = createContext(defaultValue)`.
  - Pass it to the `useContext(MyContext)` Hook to read it in any child component, no matter how deep.
  - Wrap children into `<MyContext.Provider value={...}>` to provide it from a parent.
- Context passes through any components in the middle.
- Context lets you write components that “adapt to their surroundings”.
- Before you use context, try passing props or **passing JSX as `children`**.

### Context Concepts

`createContext()` API, `Context` object, `useContext()` Hook.

#### `createContext()` API
createContext lets you create a `context` that components can provide or read.
```js
const SomeContext = createContext(defaultValue)
```

#### `Context` object
Every Context object comes with a `Provider` React component that allows consuming components to subscribe to context changes.
The Provider component accepts a **`value`** prop to be passed to consuming components that are descendants of this Provider. One Provider can be connected to many consumers. Providers can be nested to override values deeper within the tree.
All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes. 

#### `useContext()` Hook
useContext is a React Hook that lets you read and subscribe to context from your component.

```js
const value = useContext(SomeContext)
```
React automatically re-renders all the children that use a particular context starting from the provider that receives a different value. The previous and the next values are compared with the `Object.is` comparison.
`useContext()` always looks for the closest provider above the component that calls it. It searches upwards and does not consider providers in the component from which you’re calling useContext().

## Use Context

Usually you create, provide, and consume context, here it is:

#### Firstly, Create `Context.js` file

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
{% raw %}
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
{% endraw %}
How to fix it? Using global reference, like state. 

## Think more before apply context

We should apply context sparingly because it make components reuse more difficult. 
If you only want to avoid passing some props through many levels, **component composition** is often a simpler solution than context. And even **render props** is another option.

> ##### Inversion of Control
>  
> Using component composition, you can wrap the deepest component as prop, then other components don’t need to know what is passing down. This inversion of control can make your code cleaner.
{: .block-warning }

## References

https://react.dev/learn/passing-data-deeply-with-context
https://react.dev/reference/react/useContext
https://react.dev/reference/react/createContext