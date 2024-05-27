---
layout: post
title: Rendering on the Web
date: 2024-05-27
category: Website
tags:  Website Rendering React
---

There are many ways to rendering web application on the web. 
- Server-side rendering (SSR): Rendering a client-side or universal app to HTML on the server.
- Static rendering: Producing a separate HTML file for each URL ahead of time. 
- Client-side rendering (CSR): Rendering an app in a browser, using JavaScript to modify the DOM.
- Rehydration: "Booting up" JavaScript views on the client so they reuse the server-rendered HTML's DOM tree and data.
- Prerendering: Running a client-side application at build time to capture its initial state as static HTML.

Performance is the important fact for web application.
- Time to First Byte (**TTFB**): The time between clicking a link and the first byte of content loading on the new page.
- First Contentful Paint (**FCP**): The time when requested content (article body, etc) becomes visible.
- Interaction to Next Paint (**INP**): A representative metric that assesses whether a page consistently responds quickly to user inputs.
- Total Blocking Time (**TBT**): A proxy metric for INP that calculates how long the main thread was blocked during page load.

> Broadly speaking, It is encouraged to consider server-side rendering or static rendering over a full rehydration approach.
{: .block-warning}


## Server-side rendering
Server-side rendering generates the full HTML for a page on the server in response to navigation. 
This avoids additional round trips for data fetching and templating on the client, because the renderer handles them before the browser gets a response.

Server-side rendering generally produces a fast FCP.

React users can use server DOM APIs or solutions built on them like Next.js for server-side rendering. Vue users can use Vue's server-side rendering guide or Nuxt. Angular has Universal. 

## Static rendering
Static rendering happens at **build** time. With HTML responses generated in advance, you can deploy static renders to multiple CDNs to take advantage of edge caching.

Tools like Gatsby are designed to make developers feel like their application is being rendered dynamically, not generated as a build step. Static site generation tools such as 11ty, Jekyll, and Metalsmith embrace their static nature, providing a more template-driven approach.


## prerendering
static rendering and prerendering behave differently: statically rendered pages are interactive without needing to execute much client-side JavaScript, whereas prerendering improves the FCP of a Single Page Application that must be booted on the client to make pages truly interactive.

Prerendering generally needs more JavaScript to become interactive.

## Client-side rendering
Client-side rendering means rendering pages directly in the browser with JavaScript. All logic, data fetching, templating, and routing are handled on the client instead of on the server. The effective outcome is that more data is passed to the user's device from the server, and that comes with its own set of tradeoffs.

The primary downside to client-side rendering is that the amount of JavaScript required tends to grow as an application grows.


## Rehydration combines server-side and client-side rendering.

Rehydration is an approach that tries to smooth over the tradeoffs between client-side and server-side rendering by doing both. Navigation requests like full page loads or reloads are handled by a server that renders the application to HTML, then the JavaScript and data used for rendering is embedded into the resulting document. When done carefully, this achieves a fast FCP like server-side rendering, then "picks up" by rendering again on the client. 

This is an effective solution, but it can have considerable performance drawbacks.
The primary downside of server-side rendering with rehydration is that it can have a significant negative impact on TBT and INP, even if it improves FCP. 

Also, Because this duplicates a lot of HTML, rehydration can cause more problems than just delayed interactivity.

## Stream server-side rendering and rehydrate progressively
Streaming server-side rendering lets you send HTML in chunks that the browser can progressively render as it's received. This can get markup to your users faster, speeding up your FCP.

Progressive rehydration is also worth considering, and React has implemented it. With this approach, individual pieces of a server-rendered application are "booted up" over time, instead of the current common approach of initializing the entire application at once. 

## SEO considerations
When choosing a web rendering strategy, teams often consider the impact of SEO. 

Server-side rendering is a popular choice for delivering a "complete looking" experience that crawlers can interpret.

Crawlers can understand JavaScript, but there are often limitations to how they render. Client-side rendering can work, but often needs additional testing and overhead. More recently, dynamic rendering has also become an option worth considering if your architecture depends heavily on client-side JavaScript.

## Conclusion

{% include figure.html path="assets/img/app-rendering-types.png" class="img-fluid rounded z-depth-1" %}

## References

- [web.dev](https://web.dev/articles/rendering-on-the-web#seo-considerations)