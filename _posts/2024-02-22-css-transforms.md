---
layout: post
title: CSS Transforms
date: 2024-02-22
category: CSS
tags: CSS Animation 
toc:
  - name: 2D Transforms
  - name: 3D Transforms
  - name: Transition and Transform
---

There are 2D and 3D transform. 

## 2D Transforms

CSS 2D transforms allow you to move, rotate, scale, and skew elements.

Example:
```html
<style>
div {
  width: 300px;
  height: 100px;
  background-color: yellow;
  border: 1px solid black;
}
div#myDiv {
  transform: skewY(20deg);
}
</style>
<div>
This a normal div element.
</div>

<div id="myDiv">
This div element is skewed 20 degrees along the Y-axis.
</div>
```

### 2D Transform Properties
- `transform`	Applies a 2D or 3D transformation to an element
- `transform-origin` Allows you to change the position on transformed elements

### 2D Transform Methods
- `matrix(n,n,n,n,n,n)` Defines a 2D transformation, using a matrix of six values
- `translate(x,y)` Defines a 2D translation, moving the element along the X- and the Y-axis
- `translateX(n)` Defines a 2D translation, moving the element along the X-axis
- `translateY(n)` Defines a 2D translation, moving the element along the Y-axis
- `scale(x,y)` Defines a 2D scale transformation, changing the elements width and height
- `scaleX(n)` Defines a 2D scale transformation, changing the element's width
- `scaleY(n)` Defines a 2D scale transformation, changing the element's height
- `rotate(angle)` Defines a 2D rotation, the angle is specified in the parameter
- `skew(x-angle,y-angle)` Defines a 2D skew transformation along the X- and the Y-axis
- `skewX(angle)` Defines a 2D skew transformation along the X-axis
- `skewY(angle)` Defines a 2D skew transformation along the Y-axis

## 3D Transforms

### Basic Example

```html
<style>
div {
  width: 300px;
  height: 100px;
  background-color: yellow;
  border: 1px solid black;
}
#myDiv {
  transform: rotateZ(90deg);
}
</style>
<div>
This a normal div element.
</div>
<div id="myDiv">
This div element is rotated 90 degrees around the z axle.
</div>
```

### 3D transform properties:

- `transform`	Applies a 2D or 3D transformation to an element
- `transform-origin` Allows you to change the position on transformed elements
- `transform-style`	Specifies how nested elements are rendered in 3D space
- `perspective`	Specifies the perspective on how 3D elements are viewed
- `perspective-origin` Specifies the bottom position of 3D elements
- `backface-visibility`	Defines whether or not an element should be visible when not facing the screen

### 3D Transform Methods

- `matrix3d (n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)` Defines a 3D transformation, using a 4x4 matrix of 16 values
- `translate3d(x,y,z)` Defines a 3D translation
- `translateX(x)`	Defines a 3D translation, using only the value for the X-axis
- `translateY(y)`	Defines a 3D translation, using only the value for the Y-axis
- `translateZ(z)`	Defines a 3D translation, using only the value for the Z-axis
- `scale3d(x,y,z)` Defines a 3D scale transformation
- `scaleX(x)`	Defines a 3D scale transformation by giving a value for the X-axis
- `scaleY(y)`	Defines a 3D scale transformation by giving a value for the Y-axis
- `scaleZ(z)`	Defines a 3D scale transformation by giving a value for the Z-axis
- `rotate3d(x,y,z,angle)`	Defines a 3D rotation
- `rotateX(angle)` Defines a 3D rotation along the X-axis
- `rotateY(angle)` Defines a 3D rotation along the Y-axis
- `rotateZ(angle)` Defines a 3D rotation along the Z-axis
- `perspective(n)` Defines a perspective view for a 3D transformed element


## Transition and Transform

Usually, transition and transform work together to make animation.

```html
<style> 
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s, height 2s, transform 2s;
}
div:hover {
  width: 300px;
  height: 300px;
  transform: rotate(180deg);
}
</style>
<p>Hover over the div element below:</p>
<div></div>
```

## Reference

- [w3schools](https://www.w3schools.com/css/css3_transitions.asp)
