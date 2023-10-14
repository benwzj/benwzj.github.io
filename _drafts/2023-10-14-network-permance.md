---
layout: post
title: Network Performance
date: 2023-10-14
category: Website
tags: React Web-page Chrome DevTools Network
---


## Chrome DevTools

If you have clear concepts in HTML, CSS, JavaScript. Chrome DevTools is handy and easy to use. It can help you edit pages on-the-fly and diagnose problems quickly, which helps you build better websites, faster.

Here some points for starting:
- The top-level tabs are called panels. there are also have Drawer.
- right-click an element on the page and select Inspect to jump into the Elements panel which work on the DOM or CSS.
- Command+Option+J (Mac) to jump straight into the Console panel which for JavaScript. 
- Useful shortcut: command + shift + p
- Chrome DevTools provide many functionality:
  - View and Change the DOM
  - View and Change a Page's Styles   (CSS)
  - Debug JavaScript
  - View Messages and Run JavaScript in the Console
  - Optimize Website Speed
  - Inspect Network Activity

### What Chrome DevTools can do
- Debugging CSS, DevTools provide many comvenient ways for you debug CSS.
  - `inspect` your selected element, 
  - The Styles panel show all CSS declaration. good for check cascade order problem.
  - The Computed panel tell you what final sytle it use. 
  - provide many methods to search your elements.
  - provide tools to control, optimize your **animaitons**. check Animation Panel.
- Prototyping CSS, means you can edit the css and check the result. Because it is tedious to edit CSS, this function is helpful!
  - Basic idea is `inspect` your selected element, and **eidt** it.
  - Add style directly to the element.
  - Add CSS class to the elemeny.
  - Provide Contrast ratio suggestion.
  - **Copy element style**. For example you see a fancy style on a webpage, you can copy the style and make it yours. 
    1. select your fance element and `inspcet` it.
    2. Right click the element in the Element panel, and select `copy` -> `copy style`.
    3. apply it to your element.
  - Screenshots, you can screen shot whole page, your select area, ro one node in the DOM Tree, etc.
- Debugging JavaScript
  - Common problem: Check executing order; Check value are set as expected.
  - Common strategy: Logging with Console panel; Debug with source panel.
  - How to debug with source panel? Here are some hihts and tips:
    - `debugger` statement 
    - Code Stepping
    - Code folding (you can enable it!)
    - Call stck
    - Value print inline after code executed.
    - **Scope** Panel
    - Run JavaScript contextually in the Drawer(Console panel)
    - Live Expressions (turn it on by click the eye logo)
    - Add Logpoints. (just like console.log(), but don't need to write it to code)
    - Store the output of Logpoints as global variables.
  - BreakPoints: (get more info at BreakPoints Guild)
    - DOM mutation breakpoints
    - Event listener breakpoints

- Analyzing Load performance
  - Start with **Lighthouse** Panel
  - Analyze further with Coverage tag, performace panel, or Network panel depond on the resule of Lighthouse panel results.



### How to use Chrome DevTools


## Questions
- What is Site Speed Test
- What is routing, data fetching, and generating HTML
- What is Network Waterfall. How to read waterfall charts.
Network Waterfall charts show what network requests are made when loading a web page. They are often used to analyze website speed and identify opportunities for optimization. Many DevTools provide Network Waterfall charts, Like Chrome DevTools.

It provides you with a visual representation of how all the assets on your website load. This includes CSS, JavaScript, HTML, images, plugins, and third-party content.
{% include figure.html path="assets/img/waterfall-chart.png" class="img-fluid rounded z-depth-1" width="60%" %}



## Reference
- [Chrome DevTools Overview](https://developer.chrome.com/docs/devtools/overview)
- [Build better sites faster with Chrome DevTools](https://www.youtube.com/watch?v=VYyQv0CSZOE&t=2s)
