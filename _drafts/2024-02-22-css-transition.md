---
layout: post
title: CSS Transitions and Transforms
date: 2024-02-22
category: CSS
tags: CSS Animation 
toc:
  - name: CSS Transitions
  - name: Transforms
  - name: Transition and Transform
---

## CSS Transitions

CSS transitions allows you to change property values smoothly, over a given duration.

To create a transition effect, you must specify two things:
- the CSS property you want to add an effect to
- the duration of the effect

> Note: If the duration part is not specified, the transition will have no effect, because the default value is 0.

Here are some features:
- Basic idea is that, told Browser the origial properties, final properties value and duraton time, Browser can transit them smoothly. Or, There are even some options to tell browser how to transit.
- Not all properties support transitions.
- CSS transitions is one way to create animation! 
- CSS transitions and animations are **expensive operations** for most CSS properties—except `opacity` and `transform`.

### Basic example:
```html
<style> 
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s;
}

div:hover {
  width: 300px;
}
</style>
<p>Hover over the div element below, to see the transition effect:</p>
<div></div>
```

### Transition Properties
- `transition`	A shorthand property for setting the four transition properties into a single property
- `transition-delay`	Specifies a delay (in seconds) for the transition effect
- `transition-duration`	Specifies how many seconds or milliseconds a transition effect takes to complete
- `transition-property`	Specifies the name of the CSS property the transition effect is for
- `transition-timing-function`	Specifies the speed curve of the transition effect

## Transforms

There are 2D and 3D transform. 

### 2D Transforms

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

#### 2D Transform Properties
- `transform`	Applies a 2D or 3D transformation to an element
- `transform-origin` Allows you to change the position on transformed elements

#### 2D Transform Methods
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

### 3D Transforms

#### Basic Example

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

#### 3D transform properties:

- `transform`	Applies a 2D or 3D transformation to an element
- `transform-origin` Allows you to change the position on transformed elements
- `transform-style`	Specifies how nested elements are rendered in 3D space
- `perspective`	Specifies the perspective on how 3D elements are viewed
- `perspective-origin` Specifies the bottom position of 3D elements
- `backface-visibility`	Defines whether or not an element should be visible when not facing the screen

#### 3D Transform Methods

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

