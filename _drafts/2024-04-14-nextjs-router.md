---
layout: post
title: Next.js App Router Introduction
date: 2024-04-14
category: React
tags: React Next.js Router
---

## Routing Fundamentals

The skeleton of every application is routing. Next.js doc divided into two parts: App Route and Page Route. In version 13, Next.js introduced a new App Router.

## App Route vs Page Route

- App Route is new version. New Project should use App Route.
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


## The app Router
In version 13, Next.js introduced a new App Router built on React Server Components, which supports shared layouts, nested routing, loading states, error handling, and more.

The App Router works in a new directory named `app`. The `app` directory works alongside the `pages` directory to allow for incremental adoption. This allows you to opt some routes of your application into the new behavior while keeping other routes in the pages directory for previous behavior. 


## Route Handler

### What is Route Handler
Route Handlers allow you to create custom request handlers for a given route using the Web Request and Response **APIs**.

Route Handlers need to be defined in a **`route.ts`** file inside the app directory:
```ts
// app/api/route.ts
export const dynamic = 'force-dynamic' // defaults to auto
export async function GET(request: Request) {}
```
Route Handlers can be nested inside the app directory, similar to page.js and layout.js. But there cannot be a route.js file at the same route segment level as page.js.

Each route.js or page.js file takes over all HTTP verbs for that route. So they can't be at same route segment.

Route Handlers are cached by default when using the GET method with the Response object.

## FQA

### How to create dynamic route segments?
