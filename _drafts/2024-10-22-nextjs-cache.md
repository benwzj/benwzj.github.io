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
- Next.js unstable_cache
- Server Rendering Strategies
  - **Static Rendering (Default)**: With Static Rendering, routes are rendered at build time, or in the background after data revalidation. The result is cached and can be pushed to a Content Delivery Network (CDN).
  - **Dynamic Rendering**: With Dynamic Rendering, routes are rendered for each user at request time. During rendering, if a *Dynamic API* or *uncached data* request is discovered, Next.js will *switch* to dynamically rendering the whole route. 
  - **Streaming**: Streaming enables you to progressively render UI from the server. Work is split into chunks and streamed to the client as it becomes ready. This allows the user to see parts of the page immediately, before the entire content has finished rendering.
  - As a developer, you do not need to choose between static and dynamic rendering as Next.js will automatically choose the best rendering strategy for each route based on the features and APIs used. Instead, you choose when to **cache** or **revalidate** specific data, and you may choose to stream parts of your UI.
  - Dynamic APIs: like `cookies`, `headers`, or reading the incoming `searchParams` from the page props etc. which will automatically make the page render dynamically.
- RSC Payload and data are cached separately.

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

### Reusing data across multiple functions

## In-depth look at Next.js caching mechanisms

## References

- [next.js doc for caching](https://nextjs.org/docs/app/building-your-application/caching)