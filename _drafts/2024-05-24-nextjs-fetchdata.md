---
layout: post
title: Next.js Fetch data
date: 2024-05-24
category: React
tags: Next.js JavaScript React
---

## Fetching Data on the Server with `fetch`

Next.js **extends** the native `fetch` Web API to allow you to configure the caching and revalidating behavior for each fetch request on the server. So you don't need to worry fetch data many time or in many places.

React extends `fetch` to automatically memoize fetch requests while rendering a React component tree.

You can use `fetch` with `async/await` in Server Components, in Route Handlers, and in Server Actions.


