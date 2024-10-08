---
layout: post
title: Using Icon in React
date: 2024-02-19
category: React
tags: React Icon CSS
toc: 
  - name: react-icons
  - name: Font Awesome
---

## react-icons
 
'react-icons' is a plug-in which utilizes ES6 imports that allows you to include only the icons that your project is using.

Install:
```
npm install react-icons --save
```
Usage:
```js
import { FaBeer } from 'react-icons/fa';
class Question extends React.Component {
  render() {
    return <h3> Lets go for a <FaBeer />? </h3>
  }
}
```

You download all the files to your project. But This method has the trade-off that it takes a long time to install the package, and take big space.
Install:
```
npm install @react-icons/all-files --save
```
Usage:
```js
import { FaBeer } from "@react-icons/all-files/fa/FaBeer";

class Question extends React.Component {
  render() {
    return <h3> Lets go for a <FaBeer />? </h3>
  }
}
```

## Font Awesome

Use Font Awesome Kit 

- Firstly, create an account in Font Awesome, and login, then you can create Font Awesome Kit.
You can choose to create JS embeded code or CSS embeded code. Both of them are downloadable.

- Secondly, install the kit into your project: 
For example, you can copy the JS embeded code and paste it in `<scripe>` tag to the `<head>` of your index.html.
Something like this:
```html
<script
  src="https://kit.fontawesome.com/228dfc1a0b.js"
  crossorigin="anonymous"
></script>
```
- Now you can use icon in you project:
```html
<i className="fas fa-trash"></i>
```
