---
layout: post
title: Using Icon in React
date: 2024-02-19
category: React
tags: React Icon CSS
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

## fontawesome


