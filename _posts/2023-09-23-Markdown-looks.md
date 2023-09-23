---
layout: post
title: How to make MarkDown file looks better
date: 2023-09-23
tags: markdown MD CSS
category: HTML
---

We know that, there are a markdown parsers to convert markdown file into HTML. Markdown have it's own syntax for the transform. For example,` # Headline` convert to `<h1>Headline</h1>`. But how about the style?

There are two ways: 
1. Embed HTML code in markdown files (Most markdown parsers support).
2. Find a specific markdown perser.

## Embed HTML code in markdown files

- You can use inline HTML in Markdown for styles:
```html
<span style="color:green;font-weight:700;font-size:20px">
    markdown color font styles
</span>
```

- you can use CSS styles in markdown content

```markdown
<style>
.heading1 {
    color: red;
    font-weight:700;
    font-size: 35px;
}
.heading2 {
    color: blue;
    font-weight:700;
    font-size: 30px;
}
</style>

# Markdown heading styles {#identifier .heading1}
## Markdown heading styles {#identifier .heading2}

```
- Define CSS styles using selector

```markdown
<style>
red { color: red }
yellow { color: yellow }
</style>

<red> red color markdown text</red>
<yellow> red color markdown text</yellow>
```

