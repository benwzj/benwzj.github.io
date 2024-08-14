---
layout: post
title: Navigation in Single-Page App
date: 2023-01-20
category: React
tags: Navigation Vue React SPA
---

A single-page application (SPA) is a web application that re-renders its content in response to navigation actions (e.g. clicking a link) without making a request to the server to fetch new HTML.

SPA rely on the browser behavior and native APIs to enable the core functionality.

Currently, the most popular SPA frameworks are ReactJS, AngularJS, Ember.js, Vue.js, and Svelte.

## Traditional Browser Navigation

When a user click a link to make a Get request in a browser to myapp.com, the web server gonna send back a HTML file, let say index.html file. If a user makes a request to `myapp.com/dashboard`, it will send back the dashboard.html file.

This is Traditional Browser Navigation process. it is HTML-focused design.

> ##### <b>Important To Know</b>
>
> Whenever browser gets a new HTML document, all of the existing JavaScript variables, all the existing code, everything related to JavaScript is completely dumped from the current page. This is a standard browser behavior.
{: .block-warning }

## SPA Navigation

SPA will stay in a single page! it will use JavaScript to implement all the navigation precess. The first step is preventing page refresh in Browser, because this process will dump all the JavaScript context.

When users first time visit SPA website: 
- Firstly they would make a request to our server and they would get back the index.html file. 
- And That HTML file has a script tag inside of it that says the browser needs to load up the **bundle.js** file. So secondly another request would be made and we would get the bundle.js file.

SPA treat the link-clicking, or "back button" clicking differently from the traditional way:
- Stop the browser default page switching behavior. Instead handle navigation using JavaScript code.
- Figure out where the user gonna to.
- Update the content on the screen to trick users into thinking it is swapping page.
- Update address bar as well.

How SPA implement this navigation process?

### Stop the browser default page switching behavior

When you click an anchor, the browser has native behavior attached to the event to trigger navigation. However, you can also attach your own click handler and override the native behavior (using `event.preventDefault()` in modern browsers).

History API was also developed to add first-class support for single-page applications.

`window.history.pushState({},'','/dashboard')` will update the address bar, but it does not cause a full refresh of the page. 

And also if the current URL aws added by `pushState()`, When you click on the `back` or `forward` button, window emit a `popstate` event, instead of full page refreshing. 
So add event listener to this event can update your location: 
```js
window.addEventListener('popstate', event => {
  // let the router know navigation happened!
}, false);
```

### Detect address bar

React are using Browser API `window.location` to get detail of the address bar. 

For example, `window.location.pathname` can tell you where the user want to go.
{% include figure.html path="assets/img/url-parts.png" class="img-fluid rounded z-depth-1" %}

You can do this to update current path
```js
const [currentPath, setCurrentPath] = useState(window.location.pathname);
window.addEventListener('popstate', event => {
  // update curren path
  setCurrentPath(window.location.pathname)
}, false);
```

But React have already setup router for you to prepare your components for each route. You don't have to do that.

If you want to get more informatin about the process of navigation in SPA, you can check `History` and `Location` APIs.

