---
layout: post
title: CSS Animations
date: 2024-02-22
category: CSS
tags: CSS Animation
toc: 
  - name: CSS Animations
  - name: Use CSS Animations
  - name: CSS Animations vs Transition
  - name: CSS vs JS in Animation
---

CSS allows animation of HTML elements without using JavaScript!

You can use CSS to create animation by **CSS Animation** and **CSS Transition**.
But CSS Animation can create more more complex, multi-step animation.

## CSS Animations

An animation lets an element gradually change from one style to another.
You can change as many CSS properties you want, as many times as you want.

> To use CSS animation, you must first specify some keyframes for the animation. Keyframes hold what styles the element will have at certain times.
{: .block-warning}

### Basic example

Basic example show you how CSS Animations work.

The animation will last for 4 seconds, and it will gradually change the background-color of the `<div>` element from "red" to "yellow":
```css
/* The animation code */
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}
/* The element to apply the animation to */
div {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
}
```
It uses @keyframes rule, and some animation properties.

## Use CSS Animations

### animation properties

- `animation-name`: Specifies the name of the @keyframes animation
- `animation-duration`: Specifies how long time an animation should take to complete one cycle
- `animation-delay`: Specifies a delay for the start of an animation
- `animation-iteration-count`: Specifies the number of times an animation should be played
- `animation-direction`: Specifies whether an animation should be played forwards, backwards or in alternate cycles
- `animation-timing-function`: Specifies the **speed** curve of the animation.
- `animation-fill-mode`: Specifies a style for the element when the animation is not playing (before it starts, after it ends, or both)
- `animation`: A shorthand property for setting all the animation properties. Like: `animation: example 5s linear 2s infinite alternate;`

### The @keyframes Rule

When you specify CSS styles inside the `@keyframes` rule, the animation will gradually change from the current style to the new style at certain times.

Some features: 
- `@keyframes` rule is quite simple to understand. It is similar to CSS Transition.
- To get an animation to work, you must bind the animation to an element.
- JavaScript can access the `@keyframes` at-rule with the CSS object model interface `CSSKeyframesRule`.
- Properties that can't be animated in keyframe rules are ignored, but supported properties will still be animated.

Basic one like this:
```css
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}
```
same as:
```css
@keyframes example {
  0% {background-color: red;}
  100% {background-color: yellow;}
}
```

Also you can do this:
```css
@keyframes example {
  0%   {background-color: red;}
  25%  {background-color: yellow;}
  50%  {background-color: blue;}
  100% {background-color: green;}
}
```

## CSS Animations vs Transition

Both CSS transitions and animations can be used to write animation. 
Transitions are used for simple, one-step property changes, while animations are used for more complex, multi-step animations. 

They each have their own user scenarios:

- CSS transitions provide an easy way to make animations occur between the current style and an end CSS state, e.g., a resting button state and a hover state. Even if an element is in the middle of a transition, the new transition starts from the current style immediately instead of jumping to the end CSS state.
- CSS animations, on the other hand, allow developers to make animations between a set of starting property values and a final set rather than between two states. CSS animations consist of two components: a style describing the CSS animation, and a set of key frames that indicate the start and end states of the animation's style, as well as possible intermediate points. 

A little conclusion:
- In terms of performance, there is no difference between implementing an animation with CSS transitions or animations. Both of them are classified under the same CSS-based umbrella in this article.
- A transition is just one that is performed between two distinct states - i.e. a start state and an end state. Like a drawer menu, the start state could be open and the end state could be closed, or vice versa.
- If you want to perform something that does not specifically involve a start state and an end state, or you need more fine-grained control over the keyframes in a transition, then you've got to use an animation.
- Animations also have the ability to loop, play in reverse, and be controlled through JavaScript.
- transition usually Require a trigger to run (like mouse hover). 
- Transition run only one. CSS Animations can run infinitely.

## CSS vs JS in Animation

### JS Animation

The `requestAnimationFrame()` API provides an efficient way to make animations in JavaScript. The callback function of the method is called by the browser before the next repaint on each frame. Compared to `setTimeout()`/`setInterval()`, which need a specific delay parameter, `requestAnimationFrame() `is much more efficient. 
Developers can create an animation by changing an element's style each time the loop is called (or updating the Canvas draw, or whatever.)

> Note: Like CSS transitions and animations, requestAnimationFrame() pauses when the current tab is pushed into the background.

### Performance comparison
in most cases, the performance of CSS-based animations is almost the same as JavaScripted animations. 

> You can use the Browser FPS tools to test the animation performance.

### In summary
we should always try to create our animations using CSS transitions/animations where possible. If your animations are really complex, you may have to rely on JavaScript-based animations instead.
