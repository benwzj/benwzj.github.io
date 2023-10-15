---
layout: post
title: Chrome DevTools Overview
date: 2023-09-10
category: Website
tags: Web-page Chrome DevTools Network
---


If you have clear concepts in HTML, CSS, JavaScript. Chrome DevTools is handy and easy to use. Chrome DevTools is a set of web developer tools built directly into the Google Chrome browser. It can help you edit pages on-the-fly and diagnose problems quickly, which helps you build better websites, faster.

## Here some points for starting:
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

## What Chrome DevTools can do

### Debugging CSS
DevTools provide many comvenient ways for you debug CSS. Here list some main points: 
- You can `Inspect` the CSS of your selected element. 
- The __Styles__ panel show all CSS declaration. It is good for check cascade order problem.
- The __Computed__ panel tell you what final sytle it use. 
- It provide methods for you to search your elements.
- It provide tools to control, optimize your **animaitons**. check __Animation__ Panel.

### Prototyping CSS
It is tedious to edit CSS. But with Chrome DevTools, you can edit the CSS and check the result. 
- Basic idea is `inspect` your selected element, and **eidt** it.
- Add style directly to the element.
- Add CSS class to the elemeny.
- Provide Contrast ratio suggestion.
- **Copy element style**. For example you see a fancy style on a webpage, you can copy the style and make it yours. 
  1. select your fance element and `inspcet` it.
  2. Right click the element in the Element panel, and select `copy` -> `copy style`.
  3. apply it to your element.
- Screenshots, you can screen shot whole page, your select area, ro one node in the DOM Tree, etc.

### Debugging JavaScript
When you debug JavaScript, the common problems are Checking executing order; Checking value tht was set as expected.
The common strategy is Logging with Console panel. And Debugging with source panel.
How to debug with source panel? Here are some hihts and tips:
- Set `debugger` statement. (Just set breakpoint)
- Set line-of-code breakpoint
- Code Stepping.
- Code folding (you can enable it)
- Call stack
- Value print inline after code executed.
- Use **Scope** pane. When you're paused on a line of code, the Scope pane shows you what local and global variables are currently defined, along with the value of each variable. It also shows closure variables, when applicable. Double-click a variable value to edit it. 
- Run JavaScript contextually in the Drawer(Console panel)
- Live Expressions (turn it on by click the eye logo)
- Add Logpoints. (just like console.log(), but don't need to write it to code)
- Store the output of Logpoints as global variables.

### Analyzing Load performance
  - Start with **Lighthouse** Panel
  - Analyze further with Coverage tag, performace panel, or Network panel depond on the resule of Lighthouse panel results.

## Let's go a little bit deep on BreakPoints
The most well-known type of breakpoint is line-of-code. But with a large codebase it is good idea to know how and when to use the other types of breakpoints.

| Breakpoint Type	        | Use this when you want to ...                                                 |
| :---------------------- | :---------------------------------------------------------------------------- |
| Line-of-code	          | Pause on an exact region of code.                                             |
| Conditional line-of-code| Pause on an exact region of code, but only when some other condition is true. |
| Logpoint	              | Log a message to the Console without pausing the execution.                   |
| DOM	                    | Pause on the code that changes or removes a specific DOM node, or its children|
| XHR	                    | Pause when an XHR URL contains a string pattern.                              |
| Event listener	        | Pause on the code that runs after an event, such as click, is fired.          |
| Exception	              | Pause on the line of code that is throwing a caught or uncaught exception.    |
| Function	              | Pause whenever a specific function is called.                                 |
| Trusted Type	          | Pause on Trusted Type violations.                                             |


### Use a line-of-code breakpoint when you know the exact region of code that you need to investigate. 

Call `debugger` from your code to pause on that line. This is equivalent to a line-of-code breakpoint, except that the breakpoint is set in your code, not in the DevTools UI.
