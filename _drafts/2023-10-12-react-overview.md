---
layout: post
title: React Overview (update)
date: 2023-10-12
tags: React Web-page
category: React
---


Components are one of the core concepts of React.
Components are the foundation upon which you build user interfaces (UI).

The `export default` prefix lets you mark the main function in a file so that you can later import it from other files. 

## React frameworks
If you want to build a new app or a new website fully with React, use frameworks. 

### Why Frameworks
If you just want to run React, then what you need just grab react and react-dom from npm. 
But as a Web App, it still need routing, data fetching, and generating HTML for good performance.
Frameworks provide features that most apps and sites eventually need, including routing, data fetching, and generating HTML.

- **Next.js** is a full-stack React framework
- **Remix** is a full-stack React framework with nested routing
- **Gatsby** is a React framework for fast CMS-backed websites
- **Expo** (for native apps) is a React framework that lets you create universal Android, iOS, and web apps with truly native UIs. 

## root component
The React application begins at a “root” component. Usually, it is created automatically when you start a new project. For example, if you use CodeSandbox or if you use the framework Next.js, the root component is defined in pages/index.js.

## Defining a component
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!
React don't recommend to use Class components for new code.


