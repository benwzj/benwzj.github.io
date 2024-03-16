---
layout: post
title: CSS Animations
date: 2024-02-22
category: CSS
tags: CSS Animation JavaScript
toc: 
  - name: CSS Transitions
  - name: CSS Animations
  - name: Use CSS Animations
  - name: CSS Animations vs Transition
  - name: CSS vs JS in Animation
  - name: How Browser render Animation
  - name: High-performance Animations
  - name: Reference
---

CSS allows animation of HTML elements without using JavaScript!

You can use CSS to create animation by **CSS Animation** and **CSS Transition**.
But CSS Animation can create more more complex, multi-step animation.

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
- `transition`	A **shorthand** property for setting the four transition properties into a single property
- `transition-delay`	Specifies a delay (in seconds) for the transition effect
- `transition-duration`	Specifies how many seconds or milliseconds a transition effect takes to complete
- `transition-property`	Specifies the name of the CSS property the transition effect is for
- `transition-timing-function`	Specifies the speed curve of the transition effect

### Using Transitions other than `:hover`

the most common use for CSS3 Transitions has been in conjunction with the well-known CSS `:hover` pseudo-class. 

1. Transitions still can be used with some other pseudo-class.
- `:active`.
This pseudo-class matches any element that’s in the process of being activated. The primary way that an element is “activated” is by means of a mouse click, or mouse down.
- `:focus`
- `:checked`
- `:disabled`

2. Transitions Using Media Queries.
```css
.box {
  width: 440px;
  height: 440px;
  background: #222;
  margin: 0 auto;
  transition: width 2s ease, height 2s ease;
}
@media only screen and (max-width : 960px) {
  .box {
    width: 300px;
    height: 300px;
  }
}
```

3. Changing class for element using JS. The transtion works as well. 
In `Dropdown` component, when you click it, the chevron logo in right will turning up, when click again, the logo turning to normal. These are done by change class for the logo.
```css
.chevron{
  width: 1em;
  transition: transform 0.6s;
}
.chevron-up{
  width: 1em;
  transition: transform 0.6s;
  transform: rotate(180deg);
}
```

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

- CSS transitions provide an easy way to make animations occur between the current style and an end CSS state, e.g., a resting button state and a **hover** state. Even if an element is in the middle of a transition, the new transition starts from the current style immediately instead of jumping to the end CSS state.
- CSS animations, on the other hand, allow developers to make animations between a set of starting property values and a final set rather than between two states. CSS animations consist of two components: a style describing the CSS animation, and a set of key frames that indicate the start and end states of the animation's style, as well as possible intermediate points. 

A little conclusion:
- In terms of performance, there is no difference between implementing an animation with CSS transitions or animations. Both of them are classified under the same CSS-based umbrella.
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

## How Browser render Animation

Modern browsers can animate two CSS properties cheaply: `transform` and `opacity`. If you animate anything else, the chances are you're not going to hit a silky smooth 60 frames per second (FPS). Why? 
Simply say, Because this two properties is composite properties, which browser easier to handle when animate things.

Here are some concepts:
- frame rate, 
- rendering pipeline, 
- layout, 
- paint, 
- composite, 
- layer.

### Animation performance and frame rate

It is widely accepted that a frame rate of **60 FPS** is the target when animating anything on the web. This frame rate will ensure that your animations look smooth. On the web a frame is the time that it takes to do all of the work required to update and repaint the screen. If each frame does not complete within 16.7ms (1000ms / 60 ≈ 16.7), then users will perceive the delay.

### The rendering pipeline
To display something on a webpage the browser has to go through the following sequential steps:

1. Style: Calculate the styles that apply to the elements.
2. Layout: Generate the geometry and position for each element.
3. Paint: Fill out the pixels for each element into layers.
4. Composite: Draw the layers to the screen.

These four steps are known as the browser's rendering pipeline.

When you animate something on a page that has already loaded these steps have to happen again. This process begins from the step that has to be changed in order to allow the animation to take place.

As mentioned before, these steps are sequential. For example, if you animate something that changes layout, the paint and composite steps also have to run again. Animating something that changes layout is therefore more expensive than animating something that only changes compositing.

#### Animating layout properties
Layout changes involve calculating the geometry (position and size) of all the elements affected by the change. If you change one element, the geometry of other elements may need to be recalculated. For example, if you change the width of the `<html>` element any of its children may be affected. Due to the way elements overflow and affect one another, changes further down the tree can sometimes result in layout calculations all the way back up to the top.

The larger the tree of visible elements, the longer it takes to perform layout calculations.

#### Animating paint properties
Paint is the process of determining in what order elements should be painted to the screen. It is often the longest-running of all tasks in the pipeline.

The majority of painting in modern browsers is done in software rasterizers. Depending on how the elements in your app are grouped into layers, other elements besides the one that changed may also need to be painted.

#### Animating composite properties
Compositing is the process of separating the page into layers, converting the information about how the page should look into pixels (rasterization), and putting the layers together to create a page (compositing).

This is why the `opacity` property is included in the list of things which are cheap to animate. As long as this property is in its own layer, changes to it can be handled by the GPU during the compositing step. Chromium-based browsers and WebKit create a new layer for any element which has a CSS transition or animation on `opacity`.


### What is a layer?
By placing the things that will be animated or transitioned onto a new layer, the browser only needs to repaint those items and not everything else. You may be familiar with Photoshop's concept of a layer which contains a bunch of elements that can be moved together. Browser rendering layers are similar to that idea.

While the browser does a good job of making decisions about what elements should be on a new layer, if it misses one there are ways to force layer creation. You can find out about that in How to create high-performance animations. However, creating new layers should be done with care because each layer uses memory. On devices with limited memory creating new layers may cause more of a performance problem than the one you are trying to solve. In addition, each layer's textures need to be uploaded to the GPU. Therefore you may well hit constraints of bandwidth between the CPU and GPU.

### CSS vs JavaScript performance

CSS-based animations, and Web Animations (in the browsers that support the API), are typically handled on a thread known as the compositor thread. This is different from the browser's main thread, where styling, layout, painting, and JavaScript are executed. This means that if the browser is running some expensive tasks on the main thread, these animations can keep going without being interrupted.

As explained in this article, other changes to `transforms` and `opacity` can, in many cases, also be handled by the compositor thread.

If any animation triggers paint, layout, or both, the main thread will be required to do work. This is true for both CSS and JavaScript animations, and the overhead of layout or paint will likely dwarf any work associated with CSS or JavaScript execution, rendering the question moot.

## High-performance Animations

How Create High-performance Animations? There are some point need to know:

- Where possible restrict animations to **`opacity`** and **`transform`** in order to keep animations on the compositing stage of the rendering path. 
- If you must use a property that triggers layout or paint, it is unlikely that you will be able to make the animation smooth and high-performance.
- You can Use DevTools to check which stage of the path is being affected by your animations.
  - Check if an animation do layout work. Using Performance panel to check, for example in Chrome, if there are `Rendering` in the Summary tab, it may mean that your animation is causing the browser to do layout work.
  - Check if an animation triggers paint. In Chrome, Using Rendering tab ->  Paint Flashing. The [paint profiler](https://developer.chrome.com/docs/devtools/performance/reference#paint-profiler) to see if any paint operations are particularly expensive. If you find anything, see if a different CSS property will give the same look and feel with better performance.
- By placing elements on a new layer they can be repainted without also requiring the rest of the layout to be repainted. you can manually force layer creation with the [`will-change`](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change) property. But Use the `will-change` property sparingly, and only if you encounter a performance issue.

### Move an element

👎 Don't do this, because it trigger layout and paint.
```css
.box {
  position: absolute;
  top: 10px;
  left: 10px;
  animation: move 3s ease infinite;
}
@keyframes move {
  50% {
     top: calc(90vh - 160px);
     left: calc(90vw - 200px);
  }
}
```

👍 Do this:
```css
.box {
  position: absolute;
  top: 10px;
  left: 10px;
  animation: move 3s ease infinite;
}
@keyframes move {
  50% {
     transform: translate(calc(90vw - 200px), calc(90vh - 160px));
  }
}
```

Using `opacity` and `transform` you can also rotate an element, Resize an element, Change an element's visibility etc.

## Reference
- [web.dev animations overview](https://web.dev/articles/animations-overview)
- [web.dev animations guide](https://web.dev/articles/animations-guide)
- [MDN animation_performance](https://developer.mozilla.org/en-US/docs/Web/Performance/CSS_JavaScript_animation_performance)