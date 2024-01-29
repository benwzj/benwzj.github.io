---
layout: post
title: Display Modal in React
date: 2022-05-05
tags: React Motal
category: React
---

## Using ReactDOM.createPortal()

`reactjs.org` have a sample to display Modal:
1. Create Modal root Div:
```html
<div id="app-root"></div>
<div id="modal-root"></div>
```

2. Create style:
```css
#modal-root {
  position: relative;
  z-index: 999;
}
.modal {
  background-color: rgba(0,0,0,0.5);
  position: fixed;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
```
3. Then  
```js
return ReactDOM.createPortal(
  // Any valid React child: JSX, strings, arrays, etc.
  this.props.children,
  // A DOM element
  this.el,
); 
```
4. of course, you can wrap `ReactDOM.createPortal` into a component.

### how to render a modal in React
normally, rendering a modal is use z-index. But it need to take care of Stacking Context. because z-index is working differently in Stacking context. 
So in React, we need to put modal component directly child of the html body.
 
### why I can't make medal dispear when I click the background after I write some code to it's onClick? 
Because you use`useReactDOM.render()` instead of `ReactDOM.createPortal()`.

### ReactDOM.render() VS. ReactDOM.createPortal()
`ReactDOM.createPortal()`: Creates a portal. 
Portals provide a way to render children into a DOM node that exists outside the hierarchy of the DOM component.