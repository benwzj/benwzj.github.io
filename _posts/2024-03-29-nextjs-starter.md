---
layout: post
title: Next.js Starter Application
date: 2024-03-29
category: Next.js
tags: TypeScript JavaScript React Next.js
toc: 
  - name: Overview
  - name: Styling
  - name: Optimizing Fonts and Images
  - name: Layouts and Pages
  - name: Navigation
  - name: Fetch data
  - name: Streaming
  - name: Use URL search params
  - name: React Server Actions
  - name: Dynamic Route Segments
  - name: Error Handler
  - name: Form Validation
  - name: Authentication
  - name: References
---

Next.js Starter application is a Dashboard app. It is a good beginning to understand How do Next.js work and What have Next.js done.
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

The `clsx` library is one choice to toggle class names.
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
`/app/ui/fonts.ts`: 
```ts
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

`/app/layout.tsx`:
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

You can use Local font!
Import `next/font/local` and specify the `src` of your local font file. Rrecommend using variable fonts for the best performance and flexibility.


### Image Component

The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization on top of `<img>`, such as:
- Ensure image is responsive on different screen sizes.
- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.

If you are using `<img>`, you need to handle all of that yourself.
How to use `<Image>` at [here](https://nextjs.org/docs/app/building-your-application/optimizing/images).
Learn more about image at [here](https://developer.mozilla.org/en-US/docs/Learn/Performance/Multimedia) and [here](https://web.dev/learn/images).

## Layouts and Pages

Next.js uses **file-system routing** where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

> You can create separate UIs for each route using `layout.tsx` and `page.tsx` files.
{: .block-warining}

### page.tsx file

`page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible. Only the content inside the page file will be publicly accessible. 
For example, `/app/dashboard/page.tsx` is associated with the `/dashboard` path. 

### layout.tsx file

You can use a special `layout.tsx` file to create UI that is shared between multiple pages.

The idea is like this: 
layout.tsx structure:
```ts
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```
- Any components you import into `layout.tsx` file will be part of the layout.
- The `<Layout />` component receives a `children` prop. This child can either be a page or another layout.
- When `page.tsx` and `layout.tsx` are both in the same folder, the route will display `layout.tsx`.
- All the `page.tsx` files inside same folder or subfolder will automatically be nested inside a `<Layout />`.
- One benefit of using layouts is that on navigation, only the page components update while the layout won't re-render. This is called **partial rendering**.
- `/app/layout.tsx` is called a **root layout** and is required. Any UI you add to the root layout will be shared across all pages in your whole application. You can use the root layout to modify your `<html>` and `<body>` tags, and add metadata

## Navigation

### The `<Link>` Component

In Next.js, you can use the `<Link>` Component to link between pages in application. If you use `<a>` instead of `<Link>`, it will cause whole page refresh! 
`<Link>` allows you to do client-side navigation with JavaScript.
Although parts of your application are rendered on the server, hen using `<Link>` Component for navigation, there's no full page refresh, making it feel like a web app.

### Why the `<Link>` Component can do this?

It is because Next.js implement **Automatic code-splitting and prefetching**.

Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever `<Link>` components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

### Hook `usePathname()`
Next.js provide `usePathname()` to get the current link path. You can use this one to show user where are they.

## Fetch data

### How to access backend database

Some concepts: API, ORM, SQL, React Server Component;

- API: 
  - If you're using 3rd party services that provide an API.
  - If you're fetching data from the client, you want to have an API layer that runs on the server to avoid exposing your database secrets to the client.
  - API layer use ORM or SQL to fetch data.
  - In Next.js, you can create API endpoints using Route Handlers.
- ORM, like Prisma. You can use ORM to access your database. ORMs generate SQL under the hood.
- React Server Components 
If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.

### Using Server Components to fetch data!

By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:
- Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
- Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.
- Server Components execute on the server, you can query the database directly without an additional API layer.

### Parallel data fetching

Sequential fatching, also called waterfall pattern, is not necessarily bad. Sometime you may need to one step then next. However, this behavior can also be unintentional and impact performance.

A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.
In JavaScript, you can use the `Promise.all()` or `Promise.allSettled()` functions to initiate all promises at the same time.

This Parallel pattern work like this:
```ts
export async function fetchCardData() {
  try {
    const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
    const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
    const invoiceStatusPromise = sql`SELECT
         SUM(CASE WHEN status = 'paid' THEN amount ELSE 0 END) AS "paid",
         SUM(CASE WHEN status = 'pending' THEN amount ELSE 0 END) AS "pending"
         FROM invoices`;
 
    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);
    // ...
  }
}
```

> However, there is one disadvantage of relying only on this pattern: what happens if one data request is slower than all the others? Your application is only as fast as your slowest data fetch. One solution is using Streaming and static rendering.
{: .block-warning}

## Static and Dynamic Rendering

Next.js provide Static and Dynamic Rendering function. By default, when you use Server component to fetch data. Next.js provide cached function which is stastic rendering. 
Stastic rendering can make visit faster, good for SEO, reduced Server Load. 

`import { unstable_noStore as noStore } from 'next/cache';`

### What is Static Rendering
With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during revalidation. The result can then be distributed and cached in a Content Delivery Network (CDN).

Static rendering is useful for UI with no data or data that is shared across users

### What is Dynamic Rendering
With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). There are a couple of benefits of dynamic rendering:

- Real-Time Data
- User-Specific Content
- Request Time Information 

## Streaming

### What is Streaming

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

There are two ways you implement streaming in Next.js:
- At the page level, with the `loading.tsx` file.
- For specific components, with `<Suspense>`.

### Streaming Features

By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

Streaming works well with React's component model, as each component can be considered a chunk.

### Streaming a whole page with `loading.tsx`

`loading.tsx` is a special Next.js file built on top of **Suspense**, it allows you to create fallback UI to show as a replacement while page content loads. `loading.tsx` is describing the fallback UI which should be a static rendering.

To use Streaming with `loading.tsx`, the `loading.tsx` file is located at same folder level with `page.tsx`. Next.js will display `loading.tsx` first, then the `page.tsx`.

Also, The user doesn't have to wait for the page to finish loading before navigating away. (this is called interruptable navigation).

When you use `loading.tsx`, usually you use Route Groups as well.

#### Route Groups Concept

When `loading.tsx` is a level higher than `/subfolder/page.tsx` in the file system, it's also applied to that page. In order to fix this problem. Next.js use route groups concept: Create a new folder called `/(overview)` inside the dashboard folder. Then, move your `loading.tsx` and `page.tsx` files inside the folder. Now, the `loading.tsx` file will only apply to the same folder level page.

> When you create a new folder using parentheses `()`, the name won't be included in the URL path. 

### Streaming a component with `<Suspense>`

Wrap the component which need to be Streaming into `<Suspense>`.

Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

Steps: 
- Move data fetching down to the component that need it, thus isolating which parts of your routes should be dynamic in preparation for Partial Prerendering.
- Using `<Suspense>` to wrap that component.

```ts
<Suspense fallback={<FetchDataComponentSkeleton />}>
  <FetchDataComponent />
</Suspense>
```

#### Grouping Components Pattern

You can use Grouping components pattern when you want multiple components to load in at the same time. 
Step:
- Wrap all these multiple components into one component call WrapComponent.
- These Grouping components inside the WrapComponent can use `Promise.all` to parallel fetch data.
- Using `<Suspense>` to wrap that WrapComponent.

### What is Partial Prerendering

Application behaves today, where entire routes are either entirely static or dynamic.

Partial Prerendering allows you to render a route with a static loading shell, while keeping some parts dynamic. In other words, you can isolate the dynamic parts of a route. 

When a user visits a route:
- A static route shell is served, ensuring a fast initial load.
- The shell leaves holes where dynamic content will load in asynchronous.
- The async holes are streamed in parallel, reducing the overall load time of the page.

## Use URL search params

Next.js is using URL search params to manage the **search** and **pagination** state.

### Why 

- Bookmarkable and Shareable URLs
- URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering
- makes it easier to track user behavior

### How

#### Next.js provide Hooks: 
- useSearchParams - Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
- usePathname - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, usePathname would return `'/dashboard/invoices'`.
- useRouter - Enables navigation between routes within client components programmatically. 

You use these Hooks to implement the interact logic between client and server components.

#### Steps

Setup the query string Steps:

1. In client component, using `useSearchParams`, `URLSearchParams`, `useRouter` and `usePathname` to update the URL and navigate to that URL.
2. When the URL updagte, it will navigate to that route and the page for that route will works as prop update to the Server component. Server component will fetch data and re-render.

> This client component may live inside the server component in code base. This logic looks weird but works.
> So URL search params is the key point to make client and server component interact each other.
{: .block-warning}

You use **query string** to implement Fetch data, Search and Pagination function.

## React Server Actions

### What is React Server Actions

Server Actions are **asynchronous functions** that are executed on the server. They can be used in Server and Client Components to handle form submissions and data mutations in Next.js applications.

- React Server Actions allow you to run asynchronous code directly on the server to **mutate data**. You can write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

- They eliminate the need to create API endpoints to mutate data. 

- Server Actions achieve this through techniques like **`POST` requests**, **encrypted closures**, **strict input checks**, **error message hashing**, and **host restrictions**, all working together to significantly enhance your app's safety.

### How to create Server Actions

- By adding the `'use server'` to a file, you mark all the exported functions within the file as **server functions**. These server functions can then be imported into Client and Server components, making them extremely versatile.
- You can also write Server Actions directly inside Server Components by adding `"use server"` inside the action. 

Usually, you use the `action` attribute in the `<form>` element to invoke actions. When you submit the form, Server action function `createRecord` will automatically receive the native `FormData` object, containing the captured data. Like this: `createRecord(formData: FormData)`. 

> Good to know: 
>
> In HTML, you'd pass a URL to the `action` attribute of `<form>`. This URL would be the destination where your form data should be submitted (usually an API endpoint).
> However, in React, the `action` attribute of `<form>` is considered a special prop - meaning React builds on top of it to allow actions to be invoked.
> Behind the scenes, Server Actions create a `POST` API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

### Example

Invoking a Server Action within a Server Component :
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

### some point for server action

- Using `bind` to send data to server action.
- Even you just delete a record, you will wrap the delete button into a `form` to invoke server action.
- Usuall you will Revalidate and redirect after mutate data. Because of Next.js catching. 

### Revalidate and redirect

Usually, you will Revalidate and redirect in the server action.

Next.js has a Client-side Router Cache that stores the route segments in the user's browser for a time. Along with prefetching, this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.

- `revalidatePath` allows you to purge cached data on-demand for a specific path.
- The `redirect` function allows you to redirect the user to another URL. `redirect` can be used in Server Components, Route Handlers, and Server Actions.

#### How to use it

- Inside the server action function, you will add `revalidatePath('/dashboard/invoices');`. Then the `/dashboard/invoices` path will be revalidated, and fresh data will be fetched from the server.
- At this point, you also want to redirect the user back to the `/dashboard/invoices` page by adding  `redirect('/dashboard/invoices'); ` 

## Dynamic Route Segments

Next.js allows you to create Dynamic Route Segments when you don't know the exact segment name and want to create routes based on data. 
This could be product ID, blog post titles, product pages, etc. You can create dynamic route segments by wrapping a folder's name in **square brackets**. For example, [id], [post] or [slug].

For example, you need a invoice update function. Then you need to Create a Dynamic Route Segment with the invoice id. 

### How

When reading the route, according to the App directory structure, Next.js can get the value of the dynamic segment and the target `page.js`. And then Next.js will pass the vaule as parameter to target `page.js` and show the page.
- the value will correspond to square brackets segment.

### Steps:

- In your `/invoices` folder, create a new dynamic route called [id], then a new route called `edit` with a `page.tsx` file. 

- The Update Button for each invoice will do this: 

```ts
<Link
  href={`/dashboard/invoices/${id}/edit`}
  className="rounded-md border p-2 hover:bg-gray-100"
>
```

- Inside the `[id]/edit/page.tsx`, it will Read the invoice `id` from page `params`.

```ts
export default async function Page({ params }: { params: { id: string } }) {
  const id = params.id;
  // ...
}
```

- Inside the `[id]/edit/page.tsx`, you will Fetch the specific invoice according to the `id` to fill the page.  

- When user confirm update, You will update Database by using Server Action. At this moment, You can't just pass `id` to server action! Instead, you can pass `id` to the Server Action using JS bind. 

```ts
const updateInvoiceWithId = updateInvoice.bind(null, invoice.id);
return (
  <form action={updateInvoiceWithId}>
    <input type="hidden" name="id" value={invoice.id} />
  </form>
);
```

## Error Handler

Next.js works as framework, it have implement error handle flow. When something wrong happen, it can automatically implement the routine. For example, if you define a `error.tsx` file, it will turn to this file.

### `error.tsx` file

When you add `error.tsx` file to your route segments folder, the `error.tsx` file serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users.

`error.tsx` file describe the UI and should like this:

- `"use client"` - error.tsx needs to be a Client Component.
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

> That's something to keep in mind, `notFound` will take precedence over `error.tsx`, so you can reach out for it when you want to handle more specific errors!

## Form Validation

### Client-Side validation

There are a couple of ways you can validate forms on the client. The simplest would be to rely on the form validation provided by the browser by simply adding the `required` attribute to the `<input>` and `<select>` elements in your `forms`. For example:

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

- Use `useFormState` to connect form state to Server Action function.
- use `Zod` lib to implement validating form data. If form validation fails, return errors early.
- Using the `aria` labels to show error information in form.

### useFormState (useActionState)

- It is a Hook that allows you to update state based on the result of a form action.
- It is a messager between the component who use `form` and React Server **Action** function.
- Use for Server-Side Form Validation.
- Call `useFormState` at the top level of your component to create component state that is updated when a form action is invoked.
- Basic usage: like Displaying form errors, structured information after submitting a form.

> `useActionState` is the new hook to **replace** `useFormState`. `useActionState` is from `react` lib. `useFormState` hook is from `react-dom` lib. `useActionState` and `useFormState` are same.

#### How to use

```ts
const [state, formAction] = useActionState(fn, initialState, permalink?);
```

- Call `useActionState` at the top level of your component to create **component state** that is updated when a form action is invoked.
- You pass `useActionState` an existing form action function as well as an initial state, and it returns a new action that you use in your form, along with the latest form state. The latest form state is also passed to the function that you provided.

This example show you the basic usage.
```js
async function increment(previousState, formData) {
  return previousState + 1;
}

function StatefulForm({}) {
  const [state, formAction] = useActionState(increment, 0);
  return (
    <form>
      {state}
      <button formAction={formAction}>Increment</button>
    </form>
  )
}
```

### 'aria' labels 

About `aria` labels more informaiton, Check [MDN doc](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-label).

The code below will show error information when `state.errors` is `true`:
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

## Authentication

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

#### "auth.config.ts" file
Create an `auth.config.ts` file at the root of our project that exports an `authConfig` object. 
- you can Add the login pages option in this config file.
- configure Protecting your routes with Next.js Middleware.

#### "auth.ts" file
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
This login page UI should use `form` which is using `authenticate()` Server action to authenticate user.

## References

- [My github repo](https://github.com/benwzj/nextjs-demo)
- [Offical Doc](https://nextjs.org/learn/dashboard-app)
