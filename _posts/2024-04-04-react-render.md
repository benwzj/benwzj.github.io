---
layout: post
title: Rendering Process in React
date: 2024-04-04
category: React
tags: React
toc: 
  - name: Component Rendering Process
    subsections: 
      - name: How to check if a component is re-rendered
      - name: How to display render count in component
  - name: Rendering Underhood
---


Even a component render, don't means browser will repaint it.
we get component, then component instance, then element, then VDOM, then DOM.

## Component Rendering Process

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
- For subsequent renders, React will **call the function component** whose state update triggered the render.

> The default behavior of rendering will render all components nested within the updated component. 
{: .block-warning }

This might be not optimal for performance if the updated component is very high in the tree. You can use `memo` and `useMemo` to avoid re-render the child components.

#### Step 3: React commits changes to the DOM 
After rendering (calling) your components, React will modify the DOM.
- For the initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen.
- For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.

React only changes the DOM nodes if there’s a difference between renders.

After rendering is done and React updated the DOM, the browser will repaint the screen. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion with React rendering.

### How to check if a component is re-rendered

Re-render a component, means React execute that component function to calculate the output.

```js
let count = 0;
const Component = () => {
count++;
console.log("component render number: ", count);
//...
}
```

But the component might be only invoked without rerendering. 
For example, you `setState()` to the same value as previous one, React will run the component function, but without rendering the children or firing effects.

So the better way is to make a `useEffect` hook without a dependency array, this will make it run after each component render.

### How to display render count in component

You can't use State, but you can use **Ref**!!
```js
import { useRef, useEffect } from "react";
export const Counter = props => {
    const renderCounter  = useRef(0);
    useEffect (()=>{
      renderCounter.current = renderCounter.current + 1;
    });
    return <h1>Renders: {renderCounter.current}, {props.message}</h1>;
};
```

## Rendering Underhood

### React Element

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

```ts
const ele1 =<h1> Hello, GFG </h1>;
// OR
const ele1 = React.createElement('h1', {props}, "Hello, GFG")
```
The `React.createElement()` function returns an object:
```ts
{
    type: 'h1',
        props: {
        children: 'Hey Geek',
            id: 'header'
    }
}
```

Normally, React elements is returned from React components. React don't reuse element, it always destroy it and recreate it.

### Rendering an Element into the DOM

- To Render an Element into the DOM, You need a root DOM node.
```ts
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
const element = <h1>Hello, world</h1>;
root.render(element);
```
- To update the UI is to create a new element, and pass it to `root.render()`.
- ReactDom and React are same lib at version 13. React seperate them in order to support mare media, like ReactNative. 

### reconciliation
Reconciliation is responsible for maintaining the tree of elements when a components prop or state change.
Reconciliation houses the diffing algorithm that determines what parts of that tree need to be replaced. 

Here are some examples.
- When the React element's type changed, React builds a whole new tree from scratch.
- React treat DOM element and component element differently.
- For children, like list, if you insert item at the end, React will check from the start and find it is same, the second is same and so on till the last one. And it will just add one item simply. BUT, if add item at start or middle, it can be expensive! React check the first one and find the difference, then it will destroy it and build new one, and second one is different and so on. It will destroy all and build again. So, React introduce **key** prop!

### React Fiber

The actual rendering process is done by Ract Fiber.



## FQA

- How React render component with `key`?


