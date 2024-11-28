---
layout: post
title: Next.js Middleware
date: 2024-10-31
category: Next.js
tags: React TypeScript Next.js
toc: 
  - name: Overview
  - name: Understand middleware
  - name: Use Middleware in Next.js
  - name: FAQ
---

## Overview

Next.js copy the middleware concept from Express.js. 

In Next.js, Middleware allows you to run code before a request is completed. It is based on the incoming request, you can modify the response by rewriting, redirecting, modifying the request or response headers, or responding directly.

But the Next.js documents is not good enough. 

### Important Points

- Middleware will affect every request, so need to handle it carefully.
- Middleware let you share and reuse logic that is repeatable for every request.
- Middleware runs before cached content and routes are matched. 
- Middleware run for all request, Normally, you need to use `matcher` to filter "Middleware" to run on specific paths.
- The Middleware file "/middleware.ts" must export a `middleware` or a `default` function.

### What Middleware can do:
- redirect the incoming request to a different URL
- rewrite the response
- Set request headers for API Routes, getServerSideProps, and rewrite destinations
- Set response cookies
- Set response headers

### When to use Middleware:
- Authentication and Authorization
- Server-Side Redirects
- Path Rewriting, For example:  A/B testing, feature rollouts, or legacy paths 
- Bot Detection
- Logging and Analytics
- Feature Flagging


### Matching Paths
Middleware will be invoked for every route in your project. The following is the execution order:

1. headers from next.config.js
2. redirects from next.config.js
3. Middleware (rewrites, redirects, etc.)
4. beforeFiles (rewrites) from next.config.js
5. Filesystem routes (public/, _next/static/, pages/, app/, etc.)
6. afterFiles (rewrites) from next.config.js
7. Dynamic Routes (/blog/[slug])
8. fallback (rewrites) from next.config.js

There are two ways to define which paths Middleware will run on:
1. Custom matcher config
2. Conditional statements

## Understand middleware

Middleware logic happen inside the request-response cycle. 

### What is the Middleware logic 
I believe it should be this at the first thought:
Client (send Request) ----> Middleware(It do something) -----> Route page(handle the Request)
Then:
Route Page (rend back Response) ----> Middleware (then do something) ---> Client (get Response)

But after few days research, the logic maybe not like that. It may be like this: 
Client (send Request) ----> Middleware(It do something) -----> Route page(handle the Request)
Then:
Route Page (rend back Response) ----> Client (get Response)

It is easier for SW design.

### export middleware function in middleware.ts

The Middleware "/middleware" must export a `middleware` or a `default` function.

Basic example:
```ts
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

### What do the returned value of the middleware funciton means?

What i get Till now is that:
- Return `Response` or `NextResponse` object, then it will response back to client directly.
- Return `NextResponse.next()` will pass the request to the next middleware or route.
- Return `NextResponse.redirect('/login')` will redirect to `/login`.
- How about no `return`?

### Understand NextResponse

#### What do NextResponse.next() mean?
- In Middleware, The `NextResponse.next()` method allows you to return early and continue routing. What do it mean?
The next() method passes the request to the next middleware or route.

- In Middleware, The `NextResponse.next()` also allow you to forward headers when producing the response. Like this: 
```ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  // Add a new header x-current-path which passes the path to downstream components
  const headers = new Headers(request.headers);
  headers.set("x-current-path", request.nextUrl.pathname);
  return NextResponse.next({ headers });
}

export const config = {
  matcher: [
    // match all routes except static files and APIs
    "/((?!api|_next/static|_next/image|favicon.ico).*)",
  ],
};
```
we can look for the `x-current-path` header in server component:
```ts
import { headers } from "next/headers";
 
export default async function Sidebar() {
  const headerList = headers();
  const pathname = headerList.get("x-current-path");

  // ...etc
}
```

How this happen underneath? Why server component can get header information which set by `NextResponse.next`?


## Use Middleware in Next.js

- Use the file `middleware.ts` (or .js) in the **root** of your project to define Middleware.
- Only one `middleware.ts` file is supported per project
- Because Middleware will be invoked for every route in your project, so you should use `matchers` to precisely target or exclude specific routes. Or using Conditional Statements in `middleware.ts`.

### redirect
- `redirect` the incoming request to a different URL.
- `rewrite` the response by displaying a given URL. The difference between `redirect` and `rewrite` is that , `rewrite` don't show the handleing URL to user.
```ts
//middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function authenticationMiddleware(request: NextRequest) {
  // Check if the user is authenticated
  if (!request.headers.cookie?.includes('authenticated=true')) {
    return NextResponse.redirect('/login');
  }
  return NextResponse.next(); // Continue to the next Middleware or route handler
}
```

### Response to Client directly
Middleware can produce responses directly by returning a Response or NextResponse instance.
```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function customResponseMiddleware(request: NextRequest) {
  if (/* Some condition */) {
    return new Response('Custom error message', { status: 400 }); // response to Client directly
  }
  return NextResponse.next(); // pass to the next Middleware or Route handler. 
}
```

### Add header and cookie
Middleware can manipulate headers and cookies in both incoming requests and outgoing responses. This is useful for tasks like customizing the response or managing user sessions.
```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function customHeadersMiddleware(request: NextRequest) {
  const response = NextResponse.next();
  response.headers.set('x-current-path', request.nextUrl.pathname); 
  response.cookies.set('Middleware-Set-Cookie', '123');
  return response;
}
```

### Modifying Request Headers
You can use Middleware to modify the headers of outgoing API requests. This is often done to add authentication tokens, API keys, or other necessary information to the headers before sending the request.
```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function apiInterceptorMiddleware(request: NextRequest) {
  // Modify the API request headers
  request.headers.set('Authorization', 'Bearer myAccessToken');

  // Cache API responses for improved performance
  // Implement caching logic here

  return NextResponse.next();
}
```

### Caching API Responses
To improve the performance of your application, you can implement caching of API responses within Middleware. This helps reduce the load on external APIs and speeds up subsequent requests for the same data.
```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function apiInterceptorMiddleware(request: NextRequest) {
  // Check if the response is already cached
  const cachedResponse = /* Implement caching logic here */;

  if (cachedResponse) {
    // If cached, return the cached response
    return cachedResponse;
  } else {
    // If not cached, make the API request and cache the response
    const apiResponse = /* Make the API request */;

    // Cache the response for future use
    /* Cache the response */

    return apiResponse;
  }
}
```

### Mocking API Calls for Testing
Middleware can also be used to mock API calls during testing. This ensures that your tests are not dependent on external services, making them more reliable and faster.
```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function apiMockMiddleware(request: NextRequest) {
  if (process.env.NODE_ENV === 'test') {
    // In a testing environment, return a mocked response
    return new Response(JSON.stringify({ mockData: true }), {
      status: 200,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  } else {
    // In other environments, continue to the next Middleware or route handler
    return NextResponse.next();
  }
}
```

### `waitUntil` and `NextFetchEvent`

The `NextFetchEvent` object extends the native `FetchEvent` object, and includes the `waitUntil()` method.
The `waitUntil()` method takes a promise as an argument, and extends the lifetime of the Middleware until the promise settles. This is useful for performing work in the background.

```ts
// middleware.ts

import { NextResponse } from 'next/server'
import type { NextFetchEvent, NextRequest } from 'next/server'
 
export function middleware(req: NextRequest, event: NextFetchEvent) {
  event.waitUntil(
    fetch('https://my-analytics-platform.com', {
      method: 'POST',
      body: JSON.stringify({ pathname: req.nextUrl.pathname }),
    })
  )
 
  return NextResponse.next()
}
```


## FAQ

### middleware and useFormState? 
When I add following code in middleware.ts, then `useFormState` won't work propertly: the return `state` always be `undefined` which should be the returned value of the actionFn. Why?
```ts
  // Add a new header x-current-path which passes the path to downstream components
  const headers = new Headers(req.headers);
  headers.set('x-current-path', req.nextUrl.pathname);
  return NextResponse.next({ headers });
```

