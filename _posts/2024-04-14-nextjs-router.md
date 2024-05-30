---
layout: post
title: Next.js App Router Introduction
date: 2024-04-14
category: React
tags: React Next.js Router
toc: 
  - name: What is Routing
  - name: App Route vs Page Route
  - name: Route Handler
---

## What is Routing

Routing is the skeleton of every web application. It is refer to the structure of files and URL.
{% include figure.html path="assets/img/terminology-component-tree.avif" class="img-fluid rounded z-depth-1" width="80%" %}
{% include figure.html path="assets/img/terminology-url-anatomy.avif" class="img-fluid rounded z-depth-1" width="80%" %}

## App Route vs Page Route

- Next.js doc divided into two parts: App Route and Page Route.
- In version 13, Next.js introduced App Router built on React Server Components, which supports shared layouts, nested routing, loading states, error handling, and more. New Project should use App Route.
- The App Router works in a directory named `app`. The Page Route works in a `pages` directory. They can work at the same time. 
- App Route structure:
```
└── app
    ├── about
    │   └── page.tsx
    ├── globals.css
    ├── layout.tsx
    ├── login
    │   └── page.tsx
    ├── page.js 
    └── team
        └── route.tsx
```
- Page Route structure:
```
└── pages
    ├── about.tsx
    ├── index.tsx
    └── team.tsx
```
- In App Route, Components is	Server Components by default; In Page Route, component isClient Components by default.
- In App Route, `fetch` function for data fetching;	In Page Route, It is using `getServerSideProps`, `getStaticProps`, `getInitialProps`.
- In App Route, Layouts can be nested and dynamic; In Page Route, Layouts are static.

## App Route Overview

### File Conventions
Next.js provides a set of special files to create UI with specific behavior in nested routes:
- `layout`	Shared UI for a segment and its children
- `page`	Unique UI of a route and make routes publicly accessible
- `loading`	Loading UI for a segment and its children
- `not-found`	Not found UI for a segment and its children
- `error`	Error UI for a segment and its children
- `global-error`	Global Error UI
- `route`	Server-side API endpoint
- `template`	Specialized re-rendered Layout UI
- `default`	Fallback UI for Parallel Routes

### Component Hierarchy
The React components defined in special files of a route segment are rendered in a specific hierarchy:
- layout.js
- template.js
- error.js (React error boundary)
- loading.js (React suspense boundary)
- not-found.js (React error boundary)
- page.js or nested layout.js

### Page
- A `page.js` file is required to make a route segment publicly accessible.
- A page is UI that is unique to a route. You can define a page by default exporting a component from a `page.js` file.
- Pages are Server Components by default, but can be set to a Client Component.
- Pages can fetch data.

### Layouts
- A layout is UI that is **shared** between multiple routes. (HOW? the page at the same folder and all the subfolder pages share the layout!)
- On navigation, layouts preserve state, remain interactive, and do not re-render.
- You can define a layout by default exporting a React component from a `layout.js` file. The component should accept a children prop that will be populated with a child layout (if it exists) or a page during rendering.
- Root Layout (Required)
- Nesting Layouts, By default, layouts in the folder hierarchy are nested, which means they wrap child layouts via their children prop. 
- Layouts are Server Components by default but can be set to a Client Component.
- Passing data between a parent layout and its children is **not** possible. However, you can fetch the same data in a route more than once, and React will automatically dedupe the requests without affecting performance.
- Layouts do not have access to the route segments below itself. To access all route segments, you can use `useSelectedLayoutSegment` or `useSelectedLayoutSegments` in a Client Component.
- You can use `Route Groups` to opt specific route segments in and out of shared layouts.
- You can use `Route Groups` to create multiple root layouts. 

### Templates
- Templates are similar to layouts in that they wrap each child layout or page. 
- Unlike layouts that persist across routes and maintain state, templates create a new instance for each of their children on navigation. This means that when a user navigates between routes that share a template, a new instance of the component is mounted, DOM elements are recreated, state is not preserved, and effects are re-synchronized.

- Templates would be a more suitable option than layouts at below situations:
  - Features that rely on useEffect (e.g logging page views) and useState (e.g a per-page feedback form).
  - To change the default framework behavior. For example, Suspense Boundaries inside layouts only show the fallback the first time the Layout is loaded and not when switching pages. For templates, the fallback is shown on each navigation.

### Metadata
In the app directory, you can modify the `<head>` HTML elements such as title and meta using the Metadata APIs.

## Navigation

[Next.js Doc](https://nextjs.org/docs/app/building-your-application/routing)

### 4 ways to navigate between routes

- Using the `<Link>` Component: `<Link>` is a built-in component that extends the HTML `<a>` tag to provide prefetching and client-side navigation between routes. It is the primary and recommended way to navigate between routes in Next.js.
  - Linking to Dynamic Segments: ```<Link href={`/blog/${post.slug}`}>{post.title}</Link>```
  - Checking Active Links: `usePathname()` to determine if a link is active. 
- Using the `useRouter` hook (Client Components)
- Using the `redirect` function (Server Components)
- Using the native History API: 
Next.js allows you to use the native `window.history.pushState` and `window.history.replaceState` methods to update the browser's history stack without reloading the page. These APIs integrate into the Next.js Router, allowing you to sync with `usePathname` and `useSearchParams`.

### How Routing and Navigation Works
The App Router uses a hybrid approach for routing and navigation. 
- On the server, your application code is automatically **code-split** by route segments. 
- On the client, Next.js **prefetches** and **caches** the route segments. This means, when a user navigates to a new route, the browser doesn't reload the page, and only the route segments that change re-render - improving the navigation experience and performance.
- Also support Partial Rendering, Soft Navigation, Back and Forward Navigation, Routing between `pages/` and `app/`

## Streaming

- Streaming is an advanced method of Server Side Rendering.
- Streaming allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.
- Streaming implement Partial Rendering.
- This enables parts of the page to be displayed sooner, without waiting for all the data to load before any UI can be rendered.
- Streaming works well with React's component model because each component can be considered a chunk.
- Streaming is particularly beneficial when you want to prevent long data requests from blocking the page from rendering as it can reduce the Time To First Byte (TTFB) and First Contentful Paint (FCP). It also helps improve Time to Interactive (TTI),

## Route Handler

### What is Route Handler
Route Handlers allow you to create **custom request handlers** for a given route using the Web Request and Response APIs.
Route Handlers need to be defined in a **`route.ts`** file inside the app directory.

Basic Example: 
```ts
// app/api/route.ts
export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  })
  const data = await res.json()
  return Response.json({ data })
}
```
Now, any Get Request at route `api/` will be handled by the export function `GET()`.

### Route Handler Features

- Route Handlers can be nested inside the app directory, similar to page.js and layout.js.
- Each route.js or page.js file takes over all HTTP verbs for that route. So they can't be at same route segment.
- Route Handlers are cached by default when using the `GET` method with the Response object.
- In the old version, Next.js Page Route, it use "API Routes" to do the same job: providing a solution to build a public API.

### What you can do with Route Handler

- You setup Revalidate Cached Data for a Route API.
- Route Handlers can be used with dynamic functions from Next.js, like cookies and headers.
- Redirects
- Support Dynamic Route Segments
- The request object passed to the Route Handler is a NextRequest instance, which has some additional convenience methods.
- Use Streaming with Large Language Models (LLMs), such as OpenAI, for AI-generated content.
- You can use Request Body FormData.

## Middleware

Middleware allows you to run code before a request is completed. Then, based on the incoming request, you can modify the response by rewriting, redirecting, modifying the request or response headers, or responding directly.

- Middleware runs before cached content and routes are matched. 

### Middleware use cases

- Authentication and Authorization
- Server-Side Redirects
- Path Rewriting, For example:  A/B testing, feature rollouts, or legacy paths 
- Bot Detection
- Logging and Analytics
- Feature Flagging

### How to use Middelware

- Use the file `middleware.ts` (or .js) in the **root** of your project to define Middleware.
- Only one `middleware.ts` file is supported per project
- Because Middleware will be invoked for every route in your project, Use `matchers` to precisely target or exclude specific routes.

Example: 
```ts
//middleware.ts

import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}
 
// See "Matching Paths" below to learn more
export const config = {
  matcher: '/about/:path*',
}
```


## FQA

### How to create dynamic route segments?
