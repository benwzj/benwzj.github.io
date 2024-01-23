---
layout: post
title: Flex Layout is wonderful
date: 2020-04-04
category: CSS
tags: Flex CSS-Layout
toc: 
  - name: The `display` property
  - name: Flexbox layout
  - name: Flex Container Properties
  - name: Flex Item Properties
---

Before the Flexbox Layout module, there were four layout modes:

- Block, for sections in a webpage
- Inline, for text
- Table, for two-dimensional table data
- Positioned, for explicit position of an element

The Flexible Box Layout Module, makes it easier to design flexible responsive layout structure without using float or positioning.

## The `display` property

Formally, the `display` property sets an element's inner and outer display types. The outer type sets an element's participation in flow layout; the inner type sets the layout of children. 

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

## Flexbox layout

The detail of what happens when `display: flex` is declared is defined in the **CSS Flexbox layout Model** specification.
First, Container need to be `display: flex;`. And then the direct child elements of a flex container automatically becomes flexible (flex) items. 

The flex container properties are:
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

The flex item properties are:
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

## Flex Container Properties

### flex-direction Property
It specifies the direction of the flexible items.
It Can be: `row|row-reverse|column|column-reverse|initial|inherit;`

### flex-wrap property 
it specifies whether the flexible items should wrap or not.
It Can be: `nowrap|wrap|wrap-reverse|initial|inherit;`

### flex-flow property
It is a shorthand property for: `flex-direction` and `flex-wrap`

## Flex Item Properties

### flex property

The `flex` property sets the flexible length on flexible items.
It is a shorthand property for:`flex-grow; flex-shrink;flex-basis;`
Fox example: 
`flex: 1;` let all the flexible items be the same length, regardless of its content.

- flex-grow	
A number specifying how much the item will grow relative to the rest of the flexible items inside the same container.
- flex-shrink	
A number specifying how much the item will shrink relative to the rest of the flexible items
- flex-basis	
specifies the initial length of a flexible item.
Legal values: `"auto", "inherit","initial"` or a number followed by "%", "px", "em" or any other length unit




