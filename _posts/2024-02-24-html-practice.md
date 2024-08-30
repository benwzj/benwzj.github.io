---
layout: post
title: HTML Practice
date: 2024-02-24
category: HTML
tags: HTML CSS
toc: 
  - name: HTML Layout
  - name: Viewport
  - name: Metadata
  - name: text fundamentals
  - name: iframe
  - name: FAQ
---


## HTML Layout

### HTML Layout Elements
Basic sections of a document:
- `<header> `- Defines a header for a document or a section
- `<nav>` - Defines a set of navigation links
- `<section> `- Defines a section in a document
- `<article>` - Defines an independent, self-contained content
- `<aside>` - Defines content aside from the content (like a sidebar)
- `<footer>` - Defines a footer for a document or a section
- `<details>` - Defines additional details that the user can open and close on demand
- `<summary> `- Defines a heading for the `<details>` element

They are **semantic** elements.

### HTML Layout Techniques
The webpage layout is controlled by CSS.
There are four different techniques to create multicolumn layouts:
- CSS framework, like Bootstrap
- CSS float property
- CSS flexbox
- CSS grid

## Viewport

The viewport is the user's visible area of a web page and is often used when talking about mobile devices.

A more specific meaning of the term viewport refers to a meta element in HTML 5, which plays a crucial role in mobile optimization. The element scales the displayed content so that the size of the screen can be used efficiently. 

In this case, the meta element viewport ensures that all content is equally legible and displayed correctly and completely on screens of different sizes. The viewport element adapts web pages to the screen’s length and width so that mobile browsers can display the entire content correctly.

Thanks to the viewport, websites on mobile devices are not displayed in the same way as on a desktop screen. Users do not have to zoom in but can view the content of a page in a way that matches the small display. Viewports as meta elements (in combination with responsive web design) help web browsers to break up the pages and reassemble them on small screens in a way that enables users to receive a meaningful and readable image. Viewports thus have the task of preventing display problems by determining output formats that are tailored to the respective mobile device.

### Setting The Viewport
HTML5 introduced a method to let web designers take control over the viewport, through the `<meta>` tag.
You should include the following `<meta>` viewport element in all your web pages:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
This gives the browser instructions on how to control the page's dimensions and scaling.

- The `width=device-width` part sets the width of the page to follow the screen-width of the device (which will vary depending on the device).
- The `initial-scale=1.0` part sets the initial zoom level when the page is first loaded by the browser.

> Tip: If you are browsing page without this `meta` in a phone, you will have bad experience.

If you specify the viewport in a CSS file instead, you should put it right at the beginning of the file to ensure a correct display. Here's an example of how that code could look like:
```css
@viewport {
width: device-width;
}
```

### CSS Viewport Units

- `vh` stands for viewport height. This unit is based on the height of the viewport. A value of 1vh is equal to 1% of the viewport height. A value of 100vh is equal to 100% of the viewport height.
- `vw` stands for viewport width. This unit is based on the width of the viewport. A value of 1vw is equal to 1% of the viewport width.
- `vmin` stands for viewport minimum. This unit is based on the smaller dimension of the viewport. If the viewport height is smaller than the width, the value of 1vmin will be equal to 1% of the viewport height. Similarly, if the viewport width is smaller than the height, the value of 1vmin will be equal to 1% of the viewport width.
- `vmax` stands for viewport maximum. This unit is based on the larger dimension of the viewport. If the viewport height is larger than the width, the value of 1vmax will be equal to 1% of viewport height. Similarly, if the viewport width is larger than the height, the value of 1vmax will be equal to 1% of the viewport width.


## Metadata 

Metadata is data that describes data. 

In web development, metadata provides additional details about a webpage. Metadata is not visible to the users visiting the page. Instead, it works behind the scenes, embedded within the page's HTML, usually within the `<head>` element. This hidden information is crucial for search engines and other systems that need to understand your webpage's content better.

The job of `<head>` element is to contain metadata about the document. For exmaple `<title>`, `<link>`, `<script>`, `<mate>`, etc. The `<meta>` element is an "official" way of adding metadata to a document.

### Specifying your document's character encoding
`<meta charset="utf-8" />`
utf-8 is a universal character set that includes pretty much any character from any human language. This means that your web page will be able to handle displaying any language; 

> it's therefore a good idea to set `<meta charset="utf-8" />` on every web page you create! 

### Adding an author and description
```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="This is description" />
```
Good for SEO.

### Adding custom icons to your site
This metadata links the favicon (a small icon) to the webpage, displayed in the browser's address bar or tab.
`<link rel="icon" href="favicon.ico" type="image/x-icon" />`

### Setting the primary language of the document
`<html lang="en-US">`
You can also set subsections of your document to be recognized as different languages.
`<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>`

### Applying CSS and JavaScript to HTML
`<style />``<script />`

### Open Graph 
Open Graph Metadata: This metadata enhances the way a webpage is represented when shared on social media platforms, providing information such as the title, description, and preview image. Making the content more appealing and informative for users.
```html
<meta property="og:title" content="Title Here" />
<meta property="og:description" content="Description Here" />
<meta property="og:image" content="image_url_here" />
```

## text fundamentals

### structural hierarchy
HTML can be used to structure a page of text by adding headings and paragraphs, emphasizing words, creating lists.

HTML define semantics to some elements:
- There are six heading elements: h1, h2, h3, h4, h5, and h6.
- Each paragraph has to be wrapped in a `<p>` element

Using these to Implement structural hierarchy.

### `<span>` element 
It has no semantics. You use it to wrap text content when you want to apply CSS to it (or do something to it with JavaScript) without giving it any extra meaning.

### Lists
Unordered lists
Ordered lists
Description lists

### Emphasis and importance

> Here's the best rule you can remember: 
> It's only appropriate to use `<b>, <i>, or <u>` to convey a meaning traditionally conveyed with bold, italics, or underline when there isn't a more suitable element; and there usually is. Consider whether `<strong>, <em>, <mark>, or <span>` might be more appropriate.

## iframe
`<iframe>`: The Inline Frame element.
The `<iframe>` HTML element represents a nested browsing context, embedding another HTML page into the current one.


## FAQ 

- Why `scroll-behavior: smooth` not working? It is using tailwind, React and Vite.

