---
layout: post
title: Next.js Runtime
date: 2024-05-20
category: Next.js
tags: Next.js Node.js Runtime
toc: 
  - name: What is Next.js Runtime
  - name: Switch Runtime
  - name: Node.js Runtime
  - name: Edge Runtime
  - name: FAQ
  - name: References
---

## What is Next.js Runtime

In the context of Next.js, runtime refers to the set of libraries, APIs, and general functionality available to your code during execution. 

Next.js has two server runtimes to run your application: 
- The **Node.js Runtime** (default) has access to all Node.js APIs and compatible packages from the ecosystem. For doing SSR, or serving API routes. Node.js runtime can be Server or Serverless.
- The **Edge Runtime** is based on Web APIs. For Middleware.

There are many differences between these two runtimes. Here’s a quick comparison:

|  	             | Node (server) | Node (lambda)	| Edge             |
| :------------- | :-----------: | -------------: | ---------------: |
| Cold Boot	     | /             | ~250ms	        | Instant          |
| HTTP Streaming | Yes           | No             | Yes              |
| IO	           | All           | All            | fetch            |     
| Scalability	   | /             | High           | Highest          | 
| Security	     | Normal	       | High           | Highest          |
| Latency	       | Normal	       | Low	          | Lowest           |
| Code Size	     | /             | 50MB           | 1MB              |
| NPM Packages	 | All           | All	          | A smaller subset |

> You can specify a runtime for individual route segments in your Next.js application.

## Switch Runtime

### Why

Next.js' default runtime configuration is good for most use cases, but there’re still many reasons to change to one runtime over the other one. 
For example, to enable React 18’s streaming SSR feature, you cannot deploy the app to lambda. For API routes that relies on native Node.js APIs, it has to be running in the Node.js Runtime. But if an API only uses a simple cookie-based authentication, using middleware and the Edge Runtime will be a better choice due to its lower latency as well as better scalability.

### How

#### Global Runtime
A runtime option in `next.config.js`, which can be either `nodejs` or `edge` (or leave it as `undefined`):
```ts
module.exports = {
  experimental: {
    runtime?: 'nodejs' | 'edge'
  }
}
```

#### Per-route Runtime Config
For each route (pages and API routes), you can export an option runtime config such as:
```ts
export const config = {
  runtime?: 'nodejs' | 'edge'
}
```
If the segment runtime is not set, the default nodejs runtime will be used. 


## Node.js Runtime

According to the Node.js official documentation: Node.js is an asynchronous event-driven JavaScript runtime environment that lets developers create servers, web apps, command line tools and scripts.
According to the Next.js official documentation: Next.js is a React framework for building full-stack web applications. 

So Next.js is a React framework application which run in Node.js runtime environment. You can say Next.js is Node.js application.

Using the Node.js runtime gives you access to all Node.js APIs, and all npm packages that rely on them. However, it's not as fast to start up as routes using the Edge runtime.


## Edge Runtime

### What is Edge Runtime 
The Edge Runtime is designed to help framework authors adopt edge computing and provide open-source tooling built on Web standards. It’s designed to be integrated into frameworks (like Next.js) and not for usage in application code.

The Edge Runtime is a subset of Node.js APIs, giving you compatibility and interoperability between multiple web environments. The project is designed to be compliant with standards developed by WinterCG - a community group between Vercel, Cloudflare, Deno, Shopify, and more. 

> The term “Edge” refers to the orientation toward instant serverless compute environments and not a specific set of locations.
{: .block-warning }

The Edge Runtime is ideal if you need to deliver dynamic, personalized content at low latency with small, simple functions. The Edge Runtime's speed comes from its minimal use of resources,

### Using the Edge Runtime Locally
When developing and testing locally, the Edge Runtime will polyfill Web APIs and ensure compatibility with the Node.js layer.
In production, the Edge Runtime uses the JavaScript V8 engine, not Node.js, so there is no access to Node.js APIs.

## FAQ

- Where is the Edge Runtime?

## References

- [edge-runtime.vercel.app](https://edge-runtime.vercel.app/).
- [edge-and-nodejs-runtimes](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes).
- [RFC Switchable Next.js Runtime:](https://github.com/vercel/next.js/discussions/34179).