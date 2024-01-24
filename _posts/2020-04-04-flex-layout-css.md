---
layout: post
title: Flex Layout is wonderful
date: 2020-04-04
category: CSS
tags: Flex CSS-Layout
toc: 
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




