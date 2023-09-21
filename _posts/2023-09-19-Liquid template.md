---
layout: post
title: "Introduce Liquid Template"
date: 2023-09-19
categories: HTML
tags: MathJax Template
---

## What is Liquid

Liquid is an open-source template language integrated. It can be used to add dynamic content to webpages, and to create a wide variety of custom web templates.

Liquid uses a combination of **objects**, **tags**, and **filters** inside template files to display dynamic content.

It use fileters a lot, for example, using filter to get size of array.

## What is object

Objects contain the content that Liquid displays on a page. Objects and variables are displayed when enclosed in double curly braces: `{ {` and `} }`.

Liquid objects can be one of six types:
- String
- Number
- Boolean
- Nil
- Array
- EmptyDrop

You can initialize Liquid variables using assign or capture tags.

## What is tag

Tags create the logic and control flow for templates. 
The curly brace percentage delimiters `{ %` and `% }` and the text that they surround do not produce any visible output when the template is rendered. 
This lets you assign variables and create conditions or loops without showing any of the Liquid logic on the page.

## What is filter

Filters change the output of a Liquid object or variable. 
They are used within double curly braces `{ { } }` and variable assignment, and are separated by a pipe character `|`.

## ARRAYS IN LIQUID

The first element of an array is accessed with an index of 0.

Always use the “split” filter when you create an array in Liquid or else you’ll not create an array but rather a string.

```
{ %- assign example_array =  "value1, value2, value3" | split: ", " -% }

```

- Use COMPACT filter to compact an array,
- Use CONCAT filter to concatenate two arrays.
- Use FIRST filter to return the first item in an array.
- Use JOIN filter to combine all of the items in an array into a single string, separated by a space.
- Use LAST filter to return the last item in an array.
- Use the MAP filter to create a new array that contains only the specific string and store them in a new array.
- Use REVERSE filter to reverse the order of the items in an array.
- Use SIZE filter to return the size of an array.
- Use SORT filter to sort the items in an array in case-sensitive alphabetical or numerical order.
- Use SORT NATURAL filter to sort the items in an array in case-insensitive alphabetical order.
- Use SUM filter to sum all array values and returns the result.

