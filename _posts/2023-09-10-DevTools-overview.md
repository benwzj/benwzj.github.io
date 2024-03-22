---
layout: post
title: Chrome DevTools Overview
date: 2023-09-10
category: DevTools
tags: Chrome DevTools Network
giscus_comments: true
toc:
  - name: Some points for starting
  - name: What DevTools can do
  - name: Go a bit deep on BreakPoints
  - name: Panel Overview

---

If you clear the concepts in HTML, CSS, JavaScript. Chrome DevTools is handy and easy to use. Chrome DevTools is a set of web developer tools built directly into the Google Chrome browser. It can help you edit pages on-the-fly and diagnose problems quickly, which helps you build better websites, faster.

## Some points for starting

- Right-click an element on the page and select Inspect to jump into the Elements panel which work on the DOM or CSS.
- Shortcut: <keyboard>Command</keyboard>+<keyboard>Option</keyboard>+<keyboard>J</keyboard> (Mac) to jump straight into the Console panel which for JavaScript. 
- Shortcut: <keyboard>Command</keyboard>+<keyboard>shift</keyboard>+<keyboard>P</keyboard> to reach more Feature from the Drawer.
- The top-level tabs are called **Panels**. Like Console, Network, Sources, etc.
- The **Drawer** contains many hidden features. Press <keyboard>esc</keyboard> to open or close the Drawer. 
- You can Change DevTools placement, default is vertical.
- When you use DevTools a lot, you might *Customize keyboard shortcuts*.
- DevTools only logs network activity while it's open.
- Chrome DevTools provide many functionality:
  - View and Change the DOM
  - View and Change a Page's Styles (CSS)
  - Debug JavaScript
  - View Messages and Run JavaScript in the Console
  - Optimize Website Speed
  - Inspect Network Activity

## What DevTools can do

### Debugging CSS
DevTools provide many comvenient ways for you debug CSS. Here list some main points: 
- You can `Inspect` the CSS of your selected element. 
- The *Styles* panel show all CSS declaration. It is good for check cascade order problem.
- The *Computed* panel tell you what final sytle it use. 
- It provide methods for you to search your elements.
- It provide tools to control, optimize your **animaitons**. check Animation Panel.

### Prototyping CSS
It is tedious to edit CSS. But with Chrome DevTools, you can edit the CSS and check the result. 
- Basic idea is `Inspect` your selected element, and **eidt** it.
- Add style directly to the element.
- Add CSS class to the elemeny.
- Provide Contrast ratio suggestion.
- **Copy element style**. For example you see a fancy style on a webpage, you can copy the style and make it yours. 
  1. select your fance element and `Inspcet` it.
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
- Add Logpoints. (just like `console.log()`, but don't need to write it to code)
- Store the output of Logpoints as global variables.

### Analyzing Load performance
  - Start with **Lighthouse** Panel
  - Analyze further with Coverage tag, performace panel, or Network panel depond on the resule of Lighthouse panel results.

### Reference
- [Chrome DevTools Overview](https://developer.chrome.com/docs/devtools/overview)
- [Build better sites faster with Chrome DevTools](https://www.youtube.com/watch?v=VYyQv0CSZOE&t=2s)

## Go a bit deep on BreakPoints
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


### line-of-code breakpoint
Use a line-of-code breakpoint when you know the exact region of code that you need to investigate. 

Call `debugger` from your code to pause on that line. This is equivalent to a line-of-code breakpoint, except that the breakpoint is set in your code, not in the DevTools UI.

### Conditional line-of-code breakpoints
Use a conditional line-of-code breakpoint when you want to stop the execution but only when some condition is true.

Such breakpoints are useful when you want to skip breaks that are irrelevant to your case, especially in a loop.

### Log line-of-code breakpoints

Use log line-of-code breakpoints (logpoints) to log messages to the Console without pausing the execution and without cluttering up your code with `console.log()` calls.

Also, you can use the Breakpoints pane to disable, edit, or remove line-of-code breakpoints.

### DOM change breakpoints
Use a DOM change breakpoint when you want to pause on the code that changes a DOM node or its children.

To set a DOM change breakpoint:
- Click the Elements tab.
- Go to the element that you want to set the breakpoint on.
- Right-click the element.
- Hover over Break on then select Subtree modifications, Attribute modifications, or Node removal.

### XHR/fetch breakpoints
Use an XHR/fetch breakpoint when you want to break when the request URL of an XHR contains a specified string. DevTools pauses on the line of code where the XHR calls send().

One example of when this is helpful is when you see that your page is requesting an incorrect URL, and you want to quickly find the AJAX or Fetch source code that is causing the incorrect request.

To set an XHR/fetch breakpoint:
- Click the Sources tab.
- Expand the XHR Breakpoints pane.
- Click Add. Add breakpoint.
- Enter the string which you want to break on. DevTools pauses when this string is present anywhere in an XHR's request URL.
- Press Enter to confirm.

### Event listener breakpoints
Use event listener breakpoints when you want to pause on the event listener code that runs after an event is fired. You can select specific events, such as `click`, or categories of events, such as all mouse events.

- Click the Sources tab.
- Expand the Event Listener Breakpoints pane. DevTools shows a list of event categories, such as Animation.
- Check one of these categories to pause whenever any event from that category is fired, or expand the category and check a specific event.

### Exception breakpoints
Use exception breakpoints when you want to pause on the line of code that's throwing a caught or uncaught exception. You can pause on both such exceptions independently in any debug session other than Node.js.

### Function breakpoints
Call `debug(functionName)`, where `functionName` is the function you want to debug, when you want to pause whenever a specific function is called. You can insert `debug()` into your code (like a `console.log()` statement) or call it from the DevTools Console. `debug()` is equivalent to setting a line-of-code breakpoint on the first line of the function.
```js
function sum(a, b) {
  let result = a + b; // DevTools pauses on this line.
  return result;
}
debug(sum); // Pass the function object, not a string.
sum();
```

### Reference

- [Chrome DevTools breakpoints overview](https://developer.chrome.com/docs/devtools/javascript/breakpoints/)

## Panel Overview

### Elements panel & CSS

You can view **DOM nodes** in the Elements Panel.
Some skills:
- Inspect a node: 
  - Right-click ELEMENT and select Inspect.
  - Click the Inspect icon in the *top-left corner* of DevTools. And then the selected element in the viewport will be highlighted in the DOM Tree.
- Navigate the DOM Tree with a keyboard
  - Once you've selected a node in the DOM Tree, you can navigate the DOM Tree with your keyboard.
- Scroll into view:
  - When viewing the DOM Tree, right click DOM node and `Scroll into view` lets you quickly reposition the viewport so that you can see the node.
- Show rulers:
  - With rulers, you can measure the width and height of an element when you hover over it in the Elements panel.
  - Enable the rulers: Settings > Preferences > Elements > Show rulers on hover.
- Search for nodes:
  - You can search the DOM Tree by *string*, *CSS selector*, or *XPath selector*.
  - Press Control+F or Command+F (Mac). The Search bar opens at the bottom of the DOM Tree.
- Edit the DOM
  - You can edit the DOM on the fly and see how those changes affect the page.
  - In the DOM Tree, double-click the Element, see what happen.
  - Edit attributes, double-click the attribute name or value. 
  - Edit node type, double-click the type and then type in the new type.
  - Edit as HTML, select Edit as HTML from the node's drop-down menu.
  - Duplicate a node, duplicate an element using the Duplicate element right-click option.
- Capture a node screenshot
  - You can screenshot any individual node in the DOM Tree using Capture node screenshot.
  - right-click and select Capture node screenshot from the drop-down menu.
- Reorder DOM nodes
  - Drag nodes to reorder them.
- Force state
  - You can force nodes to remain in states like `:active`, `:hover`, `:focus`, `:visited`, and `:focus-within`.
  - Right-click element and select `Force State > :hover`.
- Hide a node
  - Press H to hide a node.
  - You can also right-click the node and use the Hide element option.
- Delete a node
  - Press Delete to delete a node.
- View the properties of DOM objects
  - Select a node the DOM tree, you can find Properties pane. 
  - [Check here](https://developer.chrome.com/docs/devtools/dom/properties/)
- Badges reference
  - Toggle various overlays and speed up DOM tree navigation with this comprehensive reference of badges in the Elements panel.
  - What is badge? It is small *badge sign* that can give you more information. For example, you can find `flex` badge after a `<div>`, that mean the `display` of this `<div>` is `flex`.
  - Right-click an element in the DOM tree and select Badge settings

DevTools provides a few shortcuts for accessing DOM nodes from the Console, or getting JavaScript references to them.
- Reference the currently-selected node with `$0`
  - When you inspect a node, the `== $0` text next to the node means that you can reference this node in the Console with the variable `$0`.
- Store as global variable
  - If you need to refer back to a node many times, store it as a global variable.
- Copy JS path
  - Copy the JavaScript path to a node when you need to reference it in an automated test.
  - Right-click Element in the DOM Tree and select `Copy > Copy JS Path`.

You can view and change CSS in element Panel.
- In the Elements > Styles pane!

### Console panel
The Console has 2 main uses: viewing logged messages and running JavaScript.

- `console.log('Hello, Console!')`, `console.table(array)` can display log in console.
- When building or debugging a page, it's often useful to run statements in the Console in order to change how the page looks or runs.
- Also you can Run arbitrary JavaScript that's not related to the page

### Sources panel
Use the Chrome DevTools Sources panel to:
- View files.
- Edit CSS and JavaScript.
- Create and save Snippets of JavaScript, which you can run on any page. Snippets are similar to bookmarklets.
- Debug JavaScript.
- Set up a Workspace, so that changes you make in DevTools get saved to the code on your file system.

### Network panel
You can analyze how your page load in this Network Panel!
Network Panel provides:
- Record network requests
- Change loading behavior
- Filter requests
- Sort requests
- **Analyze requests**
- Export requests data
You can know more on a [hands-on tutorial](https://developer.chrome.com/docs/devtools/network/)

### Recorder panel
Record, replay, and measure user flows.

### Performance panel
Runtime performance is how your page performs when it is running, as opposed to loading.

- You can analyze runtime performance of you website. Check [here](https://developer.chrome.com/docs/devtools/performance/)


### Memory panel
Chrome DevTools can help find memory issues that affect page performance, including memory leaks, memory bloat, and frequent garbage collections.

### Application panel
Use the Application panel to inspect, modify, and debug web app manifests, service workers, and service worker caches.

### Rendering tab
The Rendering tab helps you:
- Discover rendering performance issues. Spot repainting, layout shifts, layers and tiles, scrolling issues, see rendering statistics and Core Web Vitals.
- Emulate CSS media features. Test how pages render with different CSS media features without manually specifying them in your code or testing environment.
- Apply other useful effects. Highlight ad frames, emulate focus on a page, disable local fonts and image formats, enable an automatic dark theme, and emulate vision deficiencies.

### Reference 
- [Chrome DevTools Overview](https://developer.chrome.com/docs/devtools/overview/)

