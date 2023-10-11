---
layout: post
title: How to make MarkDown file looks better
date: 2023-09-23
tags: HTML CSS Markdown
category: Markdown
---

We know that, there are a markdown parsers to convert markdown file into HTML. Markdown have it's own syntax for the transform. For example,`#Headline` convert to `<h1>Headline</h1>`. But how about the style?

There are two ways to style your markdown file: 
1. Embed HTML code in markdown files (Most markdown parsers support).
2. Find a specific markdown perser.

## Embed HTML code in markdown files

#### inline HTML
You can use inline HTML in Markdown for styles:
```html
<span style="color:green;font-weight:700;font-size:20px">
    markdown color font styles
</span>
```
Your output looks like this:

<span style="color:green;font-weight:700;font-size:20px">
    markdown color font styles
</span>

#### Embed CSS styles
You can use CSS styles in markdown content

``` md
<style>
.heading1 {
    color: red;
    font-weight:700;
    font-size: 35px;
}
.heading3 {
    color: blue;
    font-weight:700;
    font-size: 30px;
}
</style>

# Markdown heading styles 
{: .heading1}
### Markdown heading styles 
{: .heading3}  

```
Your output looks like this:
<style>
.heading1 {
    color: red;
    font-weight:700;
    font-size: 35px;
}
.heading3 {
    color: blue;
    font-weight:700;
    font-size: 30px;
}
</style>
# Markdown heading styles 
{: .heading1}
### Markdown heading styles 
{: .heading3}  

#### Use selector
Define CSS styles using selector

```md
<style>
red { color: red }
olive { color: olive }
</style>

<red> red color markdown text</red>

<olive> olive color markdown text</olive>
```

Your output looks like this:
<style>
red { color: red }
olive { color: olive }
</style>

<red> red color markdown text</red>
<br>
<olive> olive color markdown text</olive>