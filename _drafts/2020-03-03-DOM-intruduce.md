---
layout: post
title: "Understand DOM"
date: 2020-03-03
categories: HTML
tags: Web-page JavaSript CSS Browser DOM
---

## What is the DOM

The DOM (Document Object Model) is a **programming interface** for web documents. 
- Programming interface means API.
- Web document mean web page. web page is document.

The DOM represents the document with a **logical tree**. Each branch of the tree ends in a node, and each node contains objects. That way, programming languages, e.g. JavaScrpt, can interact with the document, can change the document structure, style, and content. Nodes can have event handlers attached to them. Once an event is triggered, the event handlers get executed.

### DOM tree

In mang situation, the DOM is referred to as the DOM tree.
A DOM tree is a tree structure whose nodes represent an HTML or XML document's contents. 

> ##### NOTE
>
> Each HTML or XML document has a DOM tree representation. 
{: .block-tip}  

When a web browser parses an HTML document, it builds a DOM tree and then uses it to display the document.

Example: 
```html
<html lang="en">
  <head>
    <title>My Document</title>
  </head>
  <body>
    <h1>Header</h1>
    <p>Paragraph</p>
  </body>
</html>
```
It has a DOM tree that looks like this:
{% include figure.html path="assets/img/using_the_w3c_dom_level_1_core-doctree.jpg" class="img-fluid rounded z-depth-1" %}

### DOM API

The DOM API, also sometimes called the Document API, allows you to modify a DOM tree in any way you want.

DOM API including: 
- **core DOM**: defines the entities describing any document and the objects within it. For example, `document`, `window` are most popular interfaces to access page.
- HTML DOM API: adds support for representing HTML documents to the core DOM, 
- SVG API: adds support for representing SVG documents.

## The Terminology

### DOM and HTML source code 

It seems that HTML source code and the DOM are the same thing.
But NOT. They are totally different things. They are in defferent level. You can regard web page represented by the DOM tree. DOM tree conposite with HTML, CSS, JavaScript, etc. And DOM is the thing who implement DOM tree.

### DOM and JavaScript

DOM is not a language, you can say it is interface for language, like JavaScript, which want to interact with the page.
The DOM is not part of the JavaScript language, but is instead a Web API used to build webpage.
> ##### NOTE
>
> JavaScript is a language, DOM is the entity which are really accessing the page. And you use the language to access DOM so that you can control the document and its elements.
{: .block-tip}  

### DOM and Node
All items in the DOM are defined as nodes. There are many types of nodes, but there are three main ones that we work with most often:

- Element nodes
- Text nodes
- Comment nodes

When an HTML element is an item in the DOM, it is referred to as an element node. Any lone text outside of an element is a text node, and an HTML comment is a comment node. In addition to these three node types, the `document` itself is a document node, which is the root of all other nodes.

Every node in a document has a node type, which can be accessed through the `nodeType` property.

Below is a chart of the most common node types:

| Node           | Type Value | Example                            |
| :------------- | :--------- | :--------------------------------- |
| `ELEMENT_NODE` | 1          | The `<body>` element               |
| `TEXT_NODE`    | 3          | Text that is not part of an element|
| `COMMENT_NODE` | 8          | `<!-- an HTML comment -->`         |

### DOM and HTML element.
```html
<a href="index.html">Home</a>
```
Here we have an anchor element, which is a link to `index.html`.

- `a` is the tag
- `href` is the attribute
- `index.html` is the attribute value
- `Home` is the text.
Everything between the opening and closing tag combined make the entire HTML element.
The simplest way to access an element with JavaScript is by the `id` attribute, using the `getElementById()` method.

### DOM and Event
Every Node support specific events and can have event handlers attached to their events. Once an event is triggered, the event handlers get executed.

### DOM and Browser

Browser the software which implement DOM, run JavaScript, display page.

### DOM and Document
- DOM is Document Object Model.
- The DOM API, also sometimes called the Document API.

The Document also refer to `Document` interface.
The `Document` interface represents any web page loaded in the browser and serves as an **entry point** into the web page's content, which is the DOM tree.

The Document interface describes the common properties and methods for any kind of document. Depending on the document's type (e.g. HTML, XML, SVG, …), a larger API is available: HTML documents, served with the "text/html" content type, also implement the HTMLDocument interface, whereas XML and SVG documents implement the XMLDocument interface.

## Reference
[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
