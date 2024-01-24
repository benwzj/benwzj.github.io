---
layout: post
title: CSS Layout introduction
date: 2020-04-21
category: CSS
tags: Layout CSS
---

The `display` property is the most important CSS property for controlling layout!

## The `display` property

Formally, the `display` property sets an element's inner and outer display types. 
- The outer display type describes whether the element is block-level or inline-level. It sets an element's participation in flow layout; 
- The inner display type describes how the children of that box behave.

One of the first things we learn about CSS is that some elements are block-level and some are inline-level. These are their outer display types. For example, an `<h1>` or a `<p>` are block-level by default, and a `<span>` is inline-level. Using the `display` property we can switch between block and inline.

The [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/display) document have detail for `display` property.

For example it can be:

- `display: block` (default for block-level element). 
A block-level element is the element that always starts on a new line and takes up the full width available (stretches out to the left and right as far as it can). Like `<div> <p> <form><header><footer><section><h1>` etc. 

- `display: inline` (default for inline-level element).
An inline element does not start on a new line and only takes up as much width as necessary.
Like `<a> <img> <span> `etc. 
- `display: none` 
It is commonly used with JavaScript to hide and show elements without deleting and recreating them. 
On the other hand, `visibility:hidden;` also hides an element. However, the element will still take up the same space as before. The element will be hidden, but still affect the layout.
- `display: flex`
Displays an element as a block-level flex container
- `display: table` 
Let the element behave like a `<table>` element
- `display: inline-block `
Displays an element as an inline-level block container. 
inline is the base, this container is located inside inline level. And the container inside is block. 
The element itself is formatted as an inline element, but you can apply height and width values
When a sentence is inline, it can be broke into two lines. But when it is inline-block, it won’t be broke into two line.


