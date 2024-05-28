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

## Pages and Layouts

A page is UI that is unique to a route. You can define a page by default exporting a component from a `page.js` file.

A layout is UI that is shared between multiple routes. On navigation, layouts preserve state, remain interactive, and do not re-render.
You can define a layout by default exporting a React component from a `layout.js` file. The component should accept a children prop that will be populated with a child layout (if it exists) or a page during rendering.


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


## FQA

### How to create dynamic route segments?
