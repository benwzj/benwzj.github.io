---
layout: post
title: CSS Layout introduction
date: 2020-04-19
category: CSS
tags: Layout CSS
toc: 
  - name: Normal flow
  - name: The `display` property
  - name: Float
  - name: The `Position` property
  - name: The `z-index` property
  - name: The `overflow` property
  - name: Table layout
  - name: Multi-column layout
---

CSS control HOW you display your content in webpage. Layout is the main Skeleton of your webpage.

CSS layout allow you to take elements contained in a web page and control where they're positioned relative to the following factors: 
- their default position in normal layout flow, 
- the other elements around them, 
- their parent container, and 
- the main viewport/window. 

Here are going to introduce some concepts like: 
- normal flow, 
- display settings, 
- positioning, 
- modern layout tools like flexbox and CSS grid.

## Normal flow

Normal flow is how the browser lays out HTML pages by default when you do nothing to control page layout. 
HTML is displayed in the exact order in which it appears in the source code, with elements stacked on top of one another.

For many of the elements on your page, the normal flow will create exactly the layout you need. However, for more complex layouts you will need to alter this default behavior using some of the tools available to you in CSS. 
But Starting with a well-structured HTML document is very important because you can then work with the way things are laid out by default rather than fighting against it.

The methods that can change how elements are laid out in CSS are: The display property, float, The position property, Table layout, Multi-column layout.

## The `display` property

The standard values of `display` such as block, inline or inline-block can change how elements behave in normal flow, for example, by making a block-level element behave like an inline-level element (see Types of CSS boxes for more information). We also have entire layout methods that are enabled via specific display values, for example, CSS **Grid** and **Flexbox**, which alter how child elements are laid out inside their parents.

### multi-keyword syntax

Formally, the `display` property sets an element's inner and outer display types. 
- The **outer** display type describes whether the element is block-level or inline-level. It sets an element's participation in flow layout; 
- The **inner** display type describes how the children of that box behave.

One of the first things we learn about CSS is that some elements are block-level and some are inline-level. These are their outer display types. For example, an `<h1>` or a `<p>` are block-level by default, and a `<span>` is inline-level. Using the `display` property we can switch between block and inline.

What `grid` and `flexbox` demonstrate is that an element has both an outer and an inner display type. The outer display type describes whether the element is block-level or inline-level. The inner display type describes how the children of that box behave.

Because of this, The multi-keyword syntax is **promoted**. For example the `flex` equal to `block flex`. but we still have `inline flex`. The `block` equal to`block flow`(flow is experimental), but we still have `block grid`. The first indicate outside display, the second one indicate inside.

### Value for `display`

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

## Float

Floating an element changes the behavior of that element and the block level elements that follow it in normal flow. The floated element is moved to the left or right and removed from normal flow, and the surrounding content floats around it.

### The `float` property

- The CSS float property specifies how an element should float.
- e.g. let an image float left to the text in a container.
- float have 4 values: `none`, `left`, `right`, `inherit`

Common usage: 
`float` can be used to wrap the text around the image.
```css
img{
float: right;
}
```
It means the image with float at right. And then, the text can wrap image at left. 

### The `clear` property
- specify what elements can float beside the cleared elements and on which side.
- can be 5 values: none, left (means NO float elements allow on left side), right, both, inherit
- most common way to use clear is after you have used a float property on an element. 
- use float property, it is easy to float boxes of content side by side

For example: 
div2 just next to div1, then 
if div1 is set `float : left`, then the text in div2 will float right around of div1. usually that is not what you want;
if also set div2 `clear : left`. then the text will go under the div1.

### The `overflow` property
```css
.clearfix {
  overflow: auto;
}
```
Have same effect with follow:
```css
.clearfix::after{
  context: "";
  clear: both;
  display: table;
}
```

### box-sizing: border-box; 
An element padding and border are included in width and heigth. 
```css
* {
box-sizing: border-box;
}
```
allow this to all elements is safe and wise. 


## The `Position` property

The `position` property allows you to precisely control the placement of boxes inside other boxes. static positioning is the default in normal flow, but you can cause elements to be laid out differently using other values, for example, as fixed to the top of the browser viewport.

Totally have 5 values:

### static 
- default value. not position at special way. it is for normal flow.
- not affected by left, right, top, bottom properties. 
- It just means "put the element into its normal position in the document layout flow — nothing special to see here".

### relative
- The element is positioned relative to its normal position (static way), so "left:20px" adds 20 pixels to the element's LEFT position.
- allows you to modify an element's position on the page, moving it relative to its position in normal flow, as well as making it overlap other elements on the page.

### absolute
- moves an element completely out of the page's normal layout flow, like it's sitting on its **own separate layer**. From there, you can fix it to a position relative to the edges of its closest positioned ancestor (which becomes <html> if no other ancestors are positioned).
- The element is positioned relative to the nearest positioned(not static) ancestor (instead of positioned relative to the viewport, like fixed).
- However; if an absolute positioned element has no positioned ancestors, it uses the document body, and moves along with page scrolling.

### fixed
- The element is positioned relative to viewport, which means it always stay at the same location even the page is strolled. 
- of course, leftrighttopbottom will decide the element position.

### sticky
- makes an element act like `position: relative` until it hits a defined offset from the viewport, at which point it acts like `position: fixed`.
- The element is positioned based on the user's scroll position.
- A sticky element toggles between relative and fixed, depending on the scroll position.
- It is positioned relative until a given offset position is met in the viewport , then it "sticks" in place (like position:fixed).
- need to name top property .

## The `z-index` property

`z-index` specify the stack order of an element when overlapping. 

- only works on positioned elements (position: absolute, position: relative, position: fixed, or position: sticky).
- `z-index: -1`  means the element always at the bottom.

In the most basic cases, HTML pages can be considered two-dimensional, because text, images, and other elements are arranged on the page without overlapping. In this case, there is a single rendering flow, and all elements are aware of the space taken by others. The z-index attribute lets you adjust the order of the layering of objects when rendering content.

> In CSS 2.1, each box has a position in three dimensions. In addition to their horizontal and vertical positions, boxes lie along a "z-axis" and are formatted one on top of the other. Z-axis positions are particularly relevant when boxes overlap visually.

This means that CSS style rules allow you to position boxes on layers in addition to the default rendering layer (layer 0). The position on an imaginary z-axis of each layer is expressed as an integer representing the stacking order for rendering. Greater numbers mean closer to the observer. Control the position on this z-axis with the CSS z-index property.

Using z-index appears extremely easy: a single property assigned a single integer number with an easy-to-understand behavior.

## The `overflow` property

- specifies what should happen if content overflows an element's box.
- specifies whether to clip content or to add scrollbars when an element's content is too big to fit in a specified area.
- Note: The overflow property only works for block elements with a specified height.
- Can be visible|hidden|clip|scroll|auto|initial|inherit;

`overflow: visible` The overflow is not clipped. It renders outside the element's box. This is default
`overflow: hidden` The overflow is clipped, and the rest of the content will be invisible. Content can be scrolled programmatically (e.g. by setting scrollLeft or scrollTo())
`overflow: clip`
The overflow is clipped, and the rest of the content will be invisible. Forbids scrolling, including programmatic scrolling.	
`overflow: scroll`
The overflow is clipped, but a scroll-bar is added to see the rest of the content
`overflow: auto`
If overflow is clipped, a scroll-bar should be added to see the rest of the content
When element is taller than it's container, this property : overflow: auto can make container fit the element inside. 

## Table layout

```css
display: table; 
display: table-row;
display: table-cell;
display: table-caption;
```

Basically, they'll act like HTML table markup.

Many years ago — before even basic CSS was supported reliably across browsers — web developers used to use tables for entire web page layouts, putting their headers, footers, columns, etc. into various table rows and columns. This worked at the time, but it has many problems: table layouts are inflexible, very heavy on markup, difficult to debug, and semantically wrong (e.g., screen reader users have problems navigating table layouts).

Nowaday HTML tables are fine for displaying tabular data. 

## Multi-column layout

The multi-column layout CSS module provides us a way to lay out content in columns, similar to how text flows in a newspaper. While reading up and down columns is less useful in a web context due to the users having to scroll up and down, arranging content into columns can, nevertheless, be a useful technique.

To turn a block into a multi-column container, we use either the `column-count` property, which tells the browser how many columns we would like to have, or the `column-width` property, which tells the browser to fill the container with as many columns as possible of a specified width.

## Reference

[mdn document](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Introduction)
The [CSS_display document](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_display) have more detail for `display` property.