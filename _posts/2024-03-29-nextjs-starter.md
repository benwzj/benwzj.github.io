---
layout: post
title: Next.js Dashboard-app
date: 2024-03-29
category: React
tags: TypeScript JavaScript React Next.js
toc: 
  - name: Overview
  - name: Styling
  - name: Optimizing Fonts and Images
  - name: Layouts and Pages
  - name: Fetch data
  - name: Streaming
  - name: Search and Pagination
  - name: Server Actions
  - name: Revalidate and redirect
  - name: dynamic route segments with specific IDs
  - name: error handler
  - name: Form Validation
  - name: Adding Authentication
  - name: References
---

Next.js Starter application Dashboard-app is a good beginning to understand How do Next.js work and What have Next.js done.
This Blog list some conclusions of the application.

## Overview

- Next.js application is Node.js application.
- Next.js is powered by React.
- Next.js use Turbopack as bundler instead of Webpack.
- Next.js use SWC (Speedy Web Compiler) as compiler instead of Babel.
- About Styling: There are different ways to style your application in Next.js.
- Next.js provide some Optimizations to optimize images, links, and fonts.
- Routing is core part of Next.js: Understand how to create nested layouts and pages using **file-system** routing.
- Data Fetching: How to set up a database on Vercel, and best practices for fetching and streaming.
- Using URL Search Params for Search and Pagination 
- Mutating Data: How to mutate data using React Server Actions, and revalidate the Next.js cache.
- Provide Error Handling.
- Support server-side Form Validation and imporve Accessibility.
- Next.js provide NextAuth.js for user to add Authentication.
- Next.js provide Metadata management.

## Styling

'Tailwind CSS' and 'CSS Modules' is popular way to implement CSS in Next.js project.

### add a global CSS file to your project

It's good practice to add `global.css` to your top-level component. In Next.js, the root layout is top-level component.

### Tailwind

Next.js support Tailwind natively.
Although the CSS styles are shared globally, each class is singularly applied to each element. This means if you add or delete an element, you don't have to worry about maintaining separate stylesheets, style collisions, or the size of your CSS bundle growing as your application scales.

### CSS Modules

For example you have a css file named `home.module.css`. Then
```ts
import styles from '@/app/ui/home.module.css';
<div className={styles.shape} />;
```

### Toggle class names

You can use the `clsx` library to toggle class names.

Usage example: 
- Suppose that you want to create an InvoiceStatus component which accepts status. The status can be 'pending' or 'paid'.
- If it's 'paid', you want the color to be green. If it's 'pending', you want the color to be gray.
You can use clsx to conditionally apply the classes.

Like this:
```ts
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```

### Other styling solutions

You can also style your Next.js application with:
- **Sass** which allows you to import `.css` and `.scss` files.
- **CSS-in-JS** libraries such as `styled-jsx`, `styled-components`, and `emotion`.

## Optimizing Fonts and Images

### Fonts

Next.js **downloads** font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

Next.js do this to download font:
/app/ui/fonts.ts
```ts
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

/app/layout.tsx
```ts
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

### Images

The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization, such as:
- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.

## Layouts and Pages

Next.js uses **file-system routing** where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

You can create separate UIs for each route using `layout.tsx` and `page.tsx` files.

- `page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible. Only the content inside the page file will be publicly accessible. 
- You can use a special `layout.tsx` file to create UI that is shared between multiple pages.

### Navigating Between Pages

In Next.js, you can use the `<Link />` Component to link between pages in application. If you use <a> instead of <Link>, it will cause whole page refresh!
When using <Link />` Component, although parts of your application are rendered on the server, there's no full page refresh, making it feel like a web app.

#### Why the `<Link />` Component can do this?

It is because Next.js implment **Automatic code-splitting and prefetching**.

Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever `<Link>` components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

Also they provide `usePathname()` to get the current link path.


## Fetch data

### Using Server Components to fetch data!

By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.
As mentioned before, since Server Components execute on the server, you can query the database directly without an additional API layer.

### Static and Dynamic Rendering

Next.js provide Static and Dynamic Rendering function. By default, when you use Server component to fetch data. Next.js provide cached function which is stastic rendering. 
Stastic rendering can make visit faster, good for SEO, reduced Server Load. 

But you can use use dynamic rendering: 
`import { unstable_noStore as noStore } from 'next/cache';`

## Streaming

What is streaming?
Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

Streaming works well with React's component model, as each component can be considered a chunk.

There are two ways you implement streaming in Next.js:
- At the page level, with the loading.tsx file.
- For specific components, with `<Suspense>`.

### Streaming a whole page with `loading.tsx`

- `loading.tsx` is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.
- Since `<SideNav>` is static, it's shown immediately. The user can interact with `<SideNav>` while the dynamic content is loading.
- The user doesn't have to wait for the page to finish loading before navigating away.

### Streaming a component with `<Suspense>`

Wrap the component which need to be Streaming into `<Suspense>`.

Move data fetching down to the components that need it, thus isolating which parts of your routes should be dynamic in preparation for Partial Prerendering.
Using `<Suspense>` to wrap it.

## Search and Pagination

Next.js is using **URL search params** to manage the search state.

Next.js provide Hooks for search: 
- useSearchParams - Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
- usePathname - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, usePathname would return `'/dashboard/invoices'`.
- useRouter - Enables navigation between routes within client components programmatically. 

## Server Actions

React Server Actions allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

Server Actions achieve this through techniques like `POST` requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.

### Using forms with Server Actions

In React, you can use the action attribute in the `<form>` element to invoke actions. The action will automatically receive the native `FormData` object, containing the captured data.

For example:
```ts
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';
 
    // Logic to mutate data...
  }
 
  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

Server Actions are also deeply integrated with Next.js caching. When a form is submitted through a Server Action, not only can you use the action to mutate data, but you can also revalidate the associated cache using APIs like `revalidatePath` and `revalidateTag`.

> Good to know: 
>
> In HTML, you'd pass a URL to the action attribute of `<form>`. This URL would be the destination where your form data should be submitted (usually an API endpoint).
> However, in React, the action attribute is considered a special prop - meaning React builds on top of it to allow actions to be invoked.
> Behind the scenes, Server Actions create a `POST` API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

## Revalidate and redirect

Next.js has a Client-side Router Cache that stores the route segments in the user's browser for a time. Along with prefetching, this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.

- `revalidatePath` allows you to purge cached data on-demand for a specific path.
- The `redirect` function allows you to redirect the user to another URL. `redirect` can be used in Server Components, Route Handlers, and Server Actions.

## dynamic route segments with specific IDs

Next.js allows you to create Dynamic Route Segments when you don't know the exact segment name and want to create routes based on data. This could be blog post titles, product pages, etc. You can create dynamic route segments by wrapping a folder's name in square brackets. For example, [id], [post] or [slug].

## error handler

### `error.tsx` file

When you add `error.tsx` file to your route segments folder, the `error.tsx` file serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users.

`error.tsx` file describe the UI and should like this:

- "use client" - error.tsx needs to be a Client Component.
- It accepts two props:
  - `error`: This object is an instance of JavaScript's native Error object.
  - `reset`: This is a function to reset the error boundary. When executed, the function will try to re-render the route segment.

### `not-found.tsx` file

If you know some specific error, like not found resource, you can display specific information to user: 
1. import `notFound`:
```ts
import { notFound } from 'next/navigation';
//...
if (!invoice) {
  notFound();
}
```
2. Create a `not-found.tsx` file in your route segments folder.
3. Then `not-found.tsx` file will display when notFound happen.

> That's something to keep in mind, notFound will take precedence over error.tsx, so you can reach out for it when you want to handle more specific errors!

## Form Validation

### Client-Side validation

There are a couple of ways you can validate forms on the client. The simplest would be to rely on the form validation provided by the browser by adding the `required` attribute to the `<input>` and `<select>` elements in your `forms`. For example:

```ts
<input
  id="amount"
  name="amount"
  type="number"
  placeholder="Enter USD amount"
  className="peer block w-full rounded-md border border-gray-200 py-2 pl-10 text-sm outline-2 placeholder:text-gray-500"
  required
/>
```

### Server-Side validation

- use `Zod` to validate form data.
- If form validation fails, return errors early.
- Now you can **access the errors using the form state**. 
- Add a ternary operator that checks for each specific error. Using the `aria` labels to show message: 
```ts
  <div className="relative">
    <select
      id="customer"
      name="customerId"
      defaultValue=""
      aria-describedby="customer-error"
    >
    </select>
  </div>
  <div id="customer-error" aria-live="polite" aria-atomic="true">
    {state.errors?.customerId &&
      state.errors.customerId.map((error: string) => (
        <p className="mt-2 text-sm text-red-500" key={error}>
          {error}
        </p>
      ))}
  </div>
```

## Adding Authentication

### Basic idea like this: 

Next.js provide `NextAuth.js` to add authentication to your application. 
`NextAuth.js` abstracts away much of the complexity involved in managing sessions, sign-in and sign-out, and other aspects of authentication. It is third library. 

### How It Work

Here are the [official Doc](https://authjs.dev/reference/next-auth).

The step roughly like this:

#### Setting up NextAuth.js to your project
- install it: `npm install next-auth@beta`
- generate a secret key for your application. This key is used to encrypt cookies, ensuring the security of user sessions. Like this: `openssl rand -base64 32`
- In your `.env` file, add your generated key to the AUTH_SECRET variable: `AUTH_SECRET=your-secret-key`

#### `auth.config.ts` file
Create an `auth.config.ts` file at the root of our project that exports an `authConfig` object. 
- you can Add the login pages option in this config file.
- configure Protecting your routes with Next.js Middleware.

#### `auth.ts` file
Spreads your `authConfig` object.
```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
import Credentials from 'next-auth/providers/credentials';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [Credentials({})],
});
```

#### The login form
you create the login route and component. for example, create route `'/login'`.

## References

- [My github repo](https://github.com/benwzj/nextjs-demo)
- [Offical Doc](https://nextjs.org/learn/dashboard-app)
