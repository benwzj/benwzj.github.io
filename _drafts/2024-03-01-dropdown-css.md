---
layout: post
title: Dropdown in React
date: 2024-03-01
category: React
tags: CSS HTML React

---

## Element `<select>`
As with all form field types, `<select>` varies across browsers in its initial appearance.

The differences include box size, font-size, line-height, and most standout is the difference in how the dropdown indicator is styled.

From left to right, here is the initial appearance for `<select>` in Firefox, Chrome, and Safari:
{% include figure.html path="assets/img/select-element.png" class="img-fluid rounded z-depth-1" %}

You can create the same initial appearance across these browsers. For example using CSS, or create your own component for `<select>` function.

## CSS Style `<select>`

You can style `<select>` with CSS to provide unique apprence. 
HTML file: 
```html
<label for="standard-select">Standard Select</label>
<div class="select">
  <select id="standard-select">
    <option value="Option 1">Option 1</option>
    <option value="Option 2">Option 2</option>
    <option value="Option 3">Option 3</option>
  </select>
  <span class="focus"></span>
</div>
```
Sass file:
```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

:root {
  --select-border: #777;
  --select-focus: blue;
  --select-arrow: var(--select-border);
}

select {
  // A reset of styles, including removing the default dropdown arrow
  appearance: none;
  background-color: transparent;
  border: none;
  padding: 0 1em 0 0;
  margin: 0;
  width: 100%;
  font-family: inherit;
  font-size: inherit;
  cursor: inherit;
  line-height: inherit;

  // Stack above custom arrow
  z-index: 1;

  // Remove dropdown arrow in IE10 & IE11
  // @link https://www.filamentgroup.com/lab/select-css.html
  &::-ms-expand {
    display: none;
  }

  // Remove focus outline, will add on alternate element
  outline: none;
}

.select {
  display: grid;
  grid-template-areas: "select";
  align-items: center;
  position: relative;

  select,
  &::after {
    grid-area: select;
  }

  min-width: 15ch;
  max-width: 30ch;

  border: 1px solid var(--select-border);
  border-radius: 0.25em;
  padding: 0.25em 0.5em;

  font-size: 1.25rem;
  cursor: pointer;
  line-height: 1.1;

  // Optional styles
  // remove for transparency
  background-color: #fff;
  background-image: linear-gradient(to top, #f9f9f9, #fff 33%);

  // Custom arrow
  &:not(.select--multiple)::after {
    content: "";
    justify-self: end;
    width: 0.8em;
    height: 0.5em;
    background-color: var(--select-arrow);
    clip-path: polygon(100% 0%, 0 0%, 50% 100%);
  }
}

// Interim solution until :focus-within has better support
select:focus + .focus {
  position: absolute;
  top: -1px;
  left: -1px;
  right: -1px;
  bottom: -1px;
  border: 2px solid var(--select-focus);
  border-radius: inherit;
}
```

## React Dropdown component


## Reference

- [Using pure CSS to style `<select`>](https://moderncss.dev/custom-select-styles-with-pure-css/)
- [Here have many Styles for select](https://www.sliderrevolution.com/resources/css-select-styles/)