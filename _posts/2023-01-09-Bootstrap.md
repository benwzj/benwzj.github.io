---
layout: post
title: "Bootstrap Main points"
date: 2023-01-09
category: CSS
tags: HTML Website SASS CSS
toc: 
  - name: What is Bootstrap
  - name: Using Bootstrap 4
---

## What is Bootstrap

Bootstrap is popular HTML, CSS, and JavaScript **framework** for developing responsive, **mobile-first** websites. 

It includes HTML and CSS based design templates for typography, forms, buttons, tables, navigation, modals, image carousels and many other, as well as optional JavaScript plugins.

Bootstrap is focus on the look of single page. Mainly use CSS and partly use javascript for some effect.

Bootstrap is Writen in HTML, CSS, Less, **Sass** and JS, Originally named Twitter Blueprint.

Bootstrap 3 was released in 2013. Bootstrap 4 (released 2018) and Bootstrap 5 (released 2021).

### What Bootstrap can do

Once Bootstrap is added to a project, it provides basic style definitions for all HTML elements. 
The result is a uniform appearance for prose, tables and form elements across web browsers.
In addition, developers can take advantage of CSS classes defined in Bootstrap to further customize the appearance of their contents. 

### To use BootStrap, just take following: 
```html
  <link rel="stylesheet"
   href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
```

## Using Bootstrap 4

Main points: 
- **Containers** are used to pad the content inside of them
- **Grid** is Basic Structure
- **Flexbox** is enabled by default in 4. In general this means a move away from **floats** which in 3.

### Bootstrap 4 rely on Grid System
- is built with flexbox.
- allows up to 12 columns across the page.
- is responsive, and the columns will re-arrange automatically depending on the screen size.

### Basic Structure Example

```html
<div class="row">
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
</div>
 ```
- first star (*) represents the responsiveness: `sm`, `md`, `lg` or `xl`, 
- second star represents a number, which should add up to 12 for each row.
- `sm` means small devices - screen width `>= 576px` and `< 768px`.

### Why we need sm, md, lg, xl?
This is for helping re-arrange display according to the width of screen.
For example, when screen width is `576px`, it use `sm` class. 

It is good to explain what it is by examples:
- Code Example 1

```html
<div class="row">
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
</div>
```
When screen width is equal to or greater than `575px`, 4 columns in one row. 
When screen width is less than `575px`, it will use another class which make 4 columns stack together.
If change `col-sm-3` to `col-lg-3`, then they will stack when screen width less than `992px`

- Code Example 2

```html
<div class="container-fluid">
  <div class="row">
    <div class="col-sm-3 col-md-6">
      <p>Lorem ipsum...</p>
    </div>
    <div class="col-sm-9 col-md-6">
      <p>Sed ut perspiciatis...</p>
    </div>
  </div>
</div>
```
It will result in a 25%/75% split on small devices and a 50%/50% split on medium (and large and xlarge) devices.
The class will scales up, this means if it is 50%/50% split on medium screen, then it is 50%/50% on large and xlarge, but no on small screen.

### Some notes for Bootstrap 4

- Make sure that the sum adds up to 12 or fewer (it is not required that you use all 12 available columns)
- `<div class="row row-cols-2">` means just allow 2 columns in a row

