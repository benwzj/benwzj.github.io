---
layout: post
title: Server Component and Next.js
date: 2024-04-04
category: React
tags: React Next.js
---

Router is the skeleton of the whole Application. It can decide HOW to render Server Components and Client Components.
In Next.js, all components in the App Router are Server Components **by default**! 

## Foundational web concepts

- The **Environments** your application code can be executed in: the server and the client.
- The **Request-Response Lifecycle** that's initiated when a user visits or interacts with your application.
- The **Network Boundary** that separates server and client code.

### Rendering Environments

There are two environments where web applications can be rendered: the client and the server.

Historically, developers had to use different languages (e.g. JavaScript, PHP) and frameworks when writing code for the server and the client. With React, developers can use the same language (JavaScript), and the same framework (e.g. Next.js or your framework of choice). This flexibility allows you to seamlessly write code for both environments without context switching.

However, each environment has its own set of capabilities and constraints. Therefore, the code you write for the server and the client is not always the same. There are certain operations (e.g. data fetching or managing user state) that are better suited for one environment over the other.

### Network Boundary

In web development, the Network Boundary is a conceptual line that separates the different environments. For example, the client and the server, or the server and the data store.

In React, you choose where to place the client-server network boundary wherever it makes the most sense.

Behind the scenes, the work is split into two parts: the client module graph and the server module graph. The server module graph contains all the components that are rendered on the server, and the client module graph contains all components that are rendered on the client.

It may be helpful to think about module graphs as a visual representation of how files in your application depend on each other.

You can use the React `"use client"` convention to define the boundary. There's also a `"use server"` convention, which tells React to do some computational work on the server.

### Building Hybrid Applications

When working in these environments, it's helpful to think of the flow of the code in your application as **unidirectional**. If you need to access the server from the client, you send a new request to the server rather than re-use the same request. This makes it easier to understand where to render your components and where to place the Network Boundary.

In practice, this model encourages developers to think about what they want to execute on the server first, before sending the result to the client and making the application interactive.

## React Server Components
React Server Components allow you to write UI that can be rendered and optionally cached on the server. 

In short, Server Components allow you to do **Server Rendering**.

### Benefits of Server Rendering

- Data Fetching: Server Components allow you to move data fetching to the server.
- Security: Server Components allow you to keep sensitive data and logic on the server.
- Caching: By rendering on the server, the result can be cached and reused on subsequent requests and across users. 
- Performance: Server Components give you additional tools to optimize performance.
- Initial Page Load and First Contentful Paint (FCP).
- Search Engine Optimization and Social Network Shareability: The rendered HTML can be used by search engine bots.
- Streaming: Server Components allow you to split the rendering work into chunks and stream them to the client as they become ready. This allows the user to see parts of the page earlier without having to wait for the entire page to be rendered on the server.

### How are Server Components rendered in Next.js

Next.js uses Server Components by default. This allows you to automatically implement server rendering with no additional configuration, and you can opt into using Client Components when needed.

On the server, Next.js uses React's APIs to orchestrate rendering. The rendering work is split into chunks: 
- by individual route segments and 
- Suspense Boundaries.

Each chunk is rendered in two steps:
1. React renders Server Components into a special data format called the React Server Component Payload (**RSC Payload**).
2. Next.js uses the RSC Payload and Client Component JavaScript instructions to render HTML on the server.

Then, on the client:
1. The HTML is used to immediately show a fast non-interactive preview of the route - this is for the initial page load only.
2. The RSC Payload is used to reconcile the Client and Server Component trees, and update the DOM.
3. The JavaScript instructions are used to hydrate Client Components and make the application interactive.

### What is the React Server Component Payload (RSC)?

The RSC Payload is a compact binary representation of the rendered React Server Components tree. It's used by React on the client to update the browser's DOM. The RSC Payload contains:
- The rendered result of Server Components
- Placeholders for where Client Components should be rendered and references to their JavaScript files
- Any props passed from a Server Component to a Client Component

### Server rendering strategies
In Next.js, the rendering work is further split by route segments to enable streaming and partial rendering.

There are three subsets of server rendering: Static, Dynamic, and Streaming.

#### Static Rendering (Default)
With Static Rendering, routes are rendered at build time, or in the background after data revalidation. The result is cached and can be pushed to a Content Delivery Network (CDN). This optimization allows you to share the result of the rendering work between users and server requests.

#### Dynamic Rendering

With Dynamic Rendering, routes are rendered for each user at request time.

Dynamic rendering is useful when a route has data that is personalized to the user or has information that can only be known at request time, such as cookies or the URL's search params.

In most websites, routes are not fully static or fully dynamic - it's a spectrum. 

In Next.js, you can have dynamically rendered routes that have both cached and uncached data. This is because the RSC Payload and data are cached separately. This allows you to opt into dynamic rendering without worrying about the performance impact of fetching all the data at request time.

During rendering, if a `dynamic function` or `uncached data request` is discovered, Next.js will switch to dynamically rendering the whole route.  

In Next.js, these dynamic functions are:
- cookies() and headers(): Using these in a Server Component will opt the whole route into dynamic rendering at request time.
- searchParams: Using the searchParams prop on a Page will opt the page into dynamic rendering at request time.

#### Streaming

Streaming enables you to progressively render UI from the server. Work is split into chunks and streamed to the client as it becomes ready. This allows the user to see parts of the page immediately, before the entire content has finished rendering.

Streaming is built into the Next.js App Router by default. This helps improve both the initial page loading performance, as well as UI that depends on slower data fetches that would block rendering the whole route.


## Client Components

Client Components allow you to write interactive UI that is prerendered on the server and can use client JavaScript to run in the browser.

### Benefits of Client Rendering

- Interactivity: Client Components can use state, effects, and event listeners, meaning they can provide immediate feedback to the user and update the UI.
- Browser APIs: Client Components have access to browser APIs, like geolocation or localStorage.

### Using Client Components in Next.js
To use Client Components, you can add the React `"use client"` directive at the top of a file, above your `imports`.

`"use client"` is used to declare a boundary between a Server and Client Component modules. This means that by defining a `"use client"` in a file, all other modules imported into it, including child components, are considered part of the client bundle.