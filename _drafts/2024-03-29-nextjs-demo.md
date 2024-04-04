---
layout: post
title: Next.js Starter App Conclusion
date: 2024-03-29
category: React
tags: TypeScript JavaScript React
---

Next.js application is Node.js application.

## Main concepts

- Routing & navigation
- Metadata
- Styling: Tailwind CSS
- Image component
- Client vs Server components
- Server actions
- Suspense and streaming
- Caching
- static & dynamic rendering
- Middleware
- Folder structure
- Static export
- Deployment options
- Push to GitHub
- Environment variables in Next.js
- Hostinger VPS deployment


## Styling

### add a global CSS file 

You can import `global.css` in any component in your application, but it's usually good practice to add it to your top-level component. In Next.js, this is the root layout.

### Tailwind
Tailwind is a CSS framework that speeds up the development process by allowing you to quickly write utility classes directly in your TSX markup. Next support Tailwind natively.

### CSS Modules

```ts
import styles from '@/app/ui/home.module.css';
<div className={styles.shape} />;
```

### Using the clsx library to toggle class names

- Suppose that you want to create an InvoiceStatus component which accepts status. The status can be 'pending' or 'paid'.
- If it's 'paid', you want the color to be green. If it's 'pending', you want the color to be gray.
You can use clsx to conditionally apply the classes, like this:

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

Next.js is using URL search params to manage the search state.

Next.js provide Hooks: 
- useSearchParams - Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
- usePathname - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, usePathname would return `'/dashboard/invoices'`.
- useRouter - Enables navigation between routes within client components programmatically. 



