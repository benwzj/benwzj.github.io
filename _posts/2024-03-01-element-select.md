---
layout: post
title: The HTML Element Select
date: 2024-03-01
category: HTML
tags: CSS HTML React
toc:
  - name: Basic Usage
  - name: Use in React
  - name: Styling with CSS
  - name: React Dropdown component
  - name: Reference
---

## Basic Usage

```html
<select name="pets" id="pet-select">
  <option value="">--Please choose an option--</option>
  <option value="dog">Dog</option>
  <option value="goldfish">Goldfish</option>
</select>
```
- Each `<option>` element should have a `value` attribute containing the data value to submit to the server when that option is selected. `value` can be a object.
- You can include a `selected` attribute on an `<option>` element to make it selected by default when the page first loads.
- `size` to specify how many options should be shown at once. Default is 1.
- You can further nest `<option>` elements inside `<optgroup>` elements to create separate groups of options inside the dropdown.

Reference: 
[MDN doc](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select)

## Use in React

### Controlled and Uncontrolled
You can make a select box controlled by passing a `value` prop.
`value`: A string (or an array of strings for `multiple={true}`). Controls which option is selected. Every value string match the value of some `<option>` nested inside the `<select>`.

Caveats 
- Unlike in HTML, passing a selected attribute to `<option>` is not supported. Instead, use `<select defaultValue>` for uncontrolled select boxes and `<select value>` for controlled select boxes.
- If a select box receives a value prop, it will be treated as controlled. You must also pass an `onChange` handler that updates the passed value.
- A select box can’t be both controlled and uncontrolled at the same time.
- A select box cannot switch between being controlled or uncontrolled over its lifetime.

Reference:
[Use Select element in React](https://react.dev/reference/react-dom/components/select)

## Styling with CSS

The` <select>` element is notoriously difficult to style productively with CSS. You can affect certain aspects like any element — for example, manipulating the box model, the displayed font, etc., and you can use the appearance property to remove the default system appearance.

However, these properties don't produce a consistent result across browsers, and it is hard to do things like line different types of form element up with one another in a column. The `<select>` element's internal structure is complex, and hard to control. If you want to get full control, you should consider using a library with good facilities for styling form widgets, or try rolling your own dropdown menu using non-semantic elements, JavaScript, and WAI-ARIA to provide semantics.

From left to right, here is the initial appearance for `<select>` in Firefox, Chrome, and Safari:
{% include figure.html path="assets/img/select-element.png" class="img-fluid rounded z-depth-1" %}

You can create the same initial appearance across these browsers. For example using CSS, or create your own component for `<select>` function.

### Unique select apprence

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

You can write your own component which mimic the function with select element.
You can easy to style it with CSS. 

## Reference
- [MDN doc](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select)
- [Use Select element in React](https://react.dev/reference/react-dom/components/select)
- [Pure CSS to style `<select>`](https://moderncss.dev/custom-select-styles-with-pure-css/)
- [Styles for `<select>`](https://www.sliderrevolution.com/resources/css-select-styles/)