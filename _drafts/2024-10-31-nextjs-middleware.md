---
layout: post
title: Next.js Middleware
date: 2024-10-31
category: Next.js
tags: React TypeScript Next.js
toc: 
  - name: Features
  - name: Understand middleware
  - name: Use Middleware in Next.js
  - name: FAQ
---


Next.js copy the middleware concept from Express.js. 

In Next.js, Middleware allows you to run code before a request is completed. It is based on the incoming request, you can modify the response by rewriting, redirecting, modifying the request or response headers, or responding directly.

It can be helpful such as checking user authentication status and redirecting unauthenticated users, handling internationalization by redirecting users to locale-specific paths, or implementing custom security measures like bot detection.

## Features
- Middleware let you share and reuse logic that is repeatable for every request.
- Middleware runs before cached content and routes are matched. 
- Use `matcher` to filter "Middleware" to run on specific paths.
- What Middleware can do:
  - redirect the incoming request to a different URL
  - rewrite the response
  - Set request headers for API Routes, getServerSideProps, and rewrite destinations
  - Set response cookies
  - Set response headers
- When to use Middleware:
  - Authentication and Authorization
  - Server-Side Redirects
  - Path Rewriting, For example:  A/B testing, feature rollouts, or legacy paths 
  - Bot Detection
  - Logging and Analytics
  - Feature Flagging


## Understand middleware

Middleware logic happen inside the request-response cycle. 

### Middleware logic should be:
Client (send Request) ----> Middleware(It do something) -----> Route page(handle the Request)
Then:
Route Page (rend back Response) ----> Middleware (then do something) ---> Client (get Response)

### How Middleware handle this two directions logic?
Next.js provide `NextResponse` and `NextRequest` API to write the logic.
For example if you return `NextResponse.next();` in your middleware funciton, it means that it will pass to the next Middleware or route handler. 

### What do the returned value of the middleware funciton means?

## Understand NextResponse

### What do NextResponse.next() mean?
- In Middleware, The `NextResponse.next()` method allows you to return early and continue routing. What do it mean?
The next() method passes the request to the next middleware or route.

- In Middleware, The `NextResponse.next()` also allow you to forward headers when producing the response. What do it mean?

## Use Middleware in Next.js

- Use the file `middleware.ts` (or .js) in the **root** of your project to define Middleware.
- Only one `middleware.ts` file is supported per project
- Because Middleware will be invoked for every route in your project, so you can use `matchers` to precisely target or exclude specific routes. Or using Conditional Statements in `middleware.ts`.

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
  response.headers.set('x-current-path', request.nextUrl.pathname); // this can pass current route to server component
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

