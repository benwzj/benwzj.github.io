---
layout: post
title: "How Jekyll Theme works"
date: 2019-12-30
featured: true
categories: Website
tag: Static Website
tags: Markdown Jekyll HTML Theme

---


## Basic strategy for toggle theme
Generially, implement theme on website will do these steps:

1. Use local storage to store current theme. 
Like this:
``` javascript
localStorage.setItem("theme", theme);
```

2. Use JavaScript function to toggle theme.
There are two ways to do step:
- Use document elements `classList.toggle()` function to toggle theme.
- Or use CSS Attribute selector to toggle theme.

### Using classList.toggle()

The main structure:
```javascript
<style>
.dark-mode {
  background-color: black;
  color: white;
   p {
     color: red;
  }

}
</style>
<script>
function myFunction() {
   var element = document.body;
   element.classList.toggle("dark-mode");
}
</script>
```
Alternative way is using ` element.classList.add()` and ` element.classList.remove()` instead of `element.classList.toggle`. 

### Use CSS Attribute selector

1. First, use `variable` and `Attribute Selector` to implement website theme:

```css
[data-theme="dark"] {
  --font-size: 20px;
  --background: red;
}
```

2. Second, Using **set** and **delete** elements’ theme attribute to toggle website theme: 

```javascript
document.documentElement.setAttribute("data-theme", theme); 
document.documentElement.removeAttribute("data-theme");
```

## How Jekyll implement style on post

**Kramdown** is the default Markdown renderer for Jekyll. You can also configure Kramdown options .

**Kramdown** parse markdown file into HTML, and embed CSS style class for each HTML tag. 

### style code block
For the code block, it create `.highlight .k .n` etc. CSS class to style `<code>` part. 
- `.highlight` wrap whole `<code>` tag
- `.k` class refer to keyword:
```css
.highlight .k {
  font-weight: bold;
}
```
you can customize these classes.

## How toggle theme



