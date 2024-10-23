---
layout: post
title: Next.js Cache introduction
date: 2024-10-22
category: Next.js
tags: Next.js React Router
---

## Some concepts

- `fetch` API
- React cache
- Next.js `unstable_cache`
- Server Rendering Strategies
  - **Static Rendering (Default)**: With Static Rendering, routes are rendered at build time, or in the background after data revalidation. The result is cached and can be pushed to a Content Delivery Network (CDN).
  - **Dynamic Rendering**: With Dynamic Rendering, routes are rendered for each user at request time. During rendering, if a *Dynamic API* or *uncached data* request is discovered, Next.js will *switch* to dynamically rendering the whole route. 
  - **Streaming**: Streaming enables you to progressively render UI from the server. Work is split into chunks and streamed to the client as it becomes ready. This allows the user to see parts of the page immediately, before the entire content has finished rendering.
  - As a developer, you do not need to choose between static and dynamic rendering as Next.js will automatically choose the best rendering strategy for each route based on the features and APIs used. Instead, you choose when to **cache** or **revalidate** specific data, and you may choose to stream parts of your UI.
  - Dynamic APIs: like `cookies`, `headers`, or reading the incoming `searchParams` from the page props etc. which will automatically make the page render dynamically.
- RSC Payload and data are cached separately.

## Foundation of Next.js caching

If you use `next dev` to run the application. it won't cache the response. But when you run a production build by using `next build`, even for the server components, they will be revaluated during the build, and they will be set to be prerendered by default. (The same thing is applied for route handler.)

It can switch to Dynamic Rendering if a `Dynamic API` or `uncached data` request is discovered.

APIs and data caching affect whether a route is statically or dynamically rendered:
{% include figure.html path="assets/img/sever-rendering-switch.png" class="img-fluid rounded z-depth-1" width="80%" %}

## Example of Using Caching when fetching

### Fetching data on the server with the fetch API
If you are using `fetch`, `requests` are automatically memoized. This means you can safely call the same URL with the same options, and only one `request` will be made.
The response from fetch will be automatically cached:
`let data = await fetch('https://api.example.app/blog')`
If you do not want to cache the response from fetch, you can do the following:
`let data = await fetch('https://api.example.app/blog', { cache: 'no-store' })`

### Caching data with an ORM or Database
You can use the `unstable_cache` API to cache the response to allow pages to be prerendered when running `next build`.
```ts
import { unstable_cache } from 'next/cache'
import { db, posts } from '@/lib/db'
 
const getPosts = unstable_cache(
  async () => {
    return await db.select().from(posts)
  },
  ['posts'],
  { revalidate: 3600, tags: ['posts'] }
)
 
export default async function Page() {
  const allPosts = await getPosts()
 
  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```
This example caches the result of the database query for 1 hour (3600 seconds). It also adds the cache tag posts which can then be invalidated with Incremental Static Regeneration.


## In-depth look at Next.js caching mechanisms

Here's a high-level overview of the different caching mechanisms and their purpose:
{% include figure.html path="assets/img/caching-overview.png" class="img-fluid rounded z-depth-1" width="80%" %}

By default, Next.js will cache as much as possible to improve performance and reduce cost. This means routes are statically rendered and data requests are cached unless you opt out. 

### Request Memoization
Request memoization is a React feature, not a Next.js feature. 
React extends the `fetch` API to automatically memoize requests that have the same URL and options. This means you can call a fetch function for the same data in multiple places in a React component tree while only executing it once.

The memoization is not shared across server requests and only applies during rendering, there is no need to revalidate it.

### Data Cache
Next.js has a built-in Data Cache that persists the result of data fetches across incoming server requests and deployments. This is possible because Next.js extends the native fetch API to allow each request on the server to set its own persistent caching semantics.

> Good to know: In the browser, the `cache` option of `fetch` indicates how a request will interact with the browser's HTTP cache, in Next.js, the `cache` option indicates how a server-side request will interact with the server's Data Cache.

The Data Cache is persistent across incoming requests and deployments unless you revalidate or opt-out.
- revalidate:
  - Time-based Revalidation: `fetch('https://api.example.app/blog', { next: { revalidate: 3600 } })`
  - On-demand Revalidation: Data can be revalidated on-demand by path (`revalidatePath`) or by cache tag (`revalidateTag`).
- opt-out: using `let data = await fetch('https://api.example.app/blog', { cache: 'no-store' })`.

#### Differences between the Data Cache and Request Memoization

While both caching mechanisms help improve performance by re-using cached data, the Data Cache is persistent across incoming requests and deployments, whereas memoization only lasts the lifetime of a request.

### Full Route Cache

Next.js automatically renders and caches routes at build time. This is an optimization that allows you to serve the cached route instead of rendering on the server for every request, resulting in faster page loads.

To understand how the Full Route Cache works, it's helpful to look at how React handles rendering, and how Next.js caches the result:

1. React Rendering on the Server: The rendering work is split into chunks: by individual routes segments and Suspense boundaries.
2. Next.js Caching on the Server (Full Route Cache): The default behavior of Next.js is to cache the rendered result (React Server Component Payload and HTML) of a route on the server. This applies to statically rendered routes at build time, or during revalidation.
3. React Hydration and Reconciliation on the Client: At request time, on the client:
  - The HTML is used to **immediately show** a fast non-interactive initial preview of the Client and Server Components.
  - The RSC Payload is used to **reconcile** the Client and rendered Server Component trees, and update the DOM.
  - The JavaScript instructions are used to **hydrate** Client Components and make the application interactive.
4. Next.js Caching on the Client (Router Cache)
5. Subsequent Navigations

## References

- [next.js doc for caching](https://nextjs.org/docs/app/building-your-application/caching)