---
layout: post
title: About Auth.js
date: 2024-06-13
category: Next.js
tags: Next.js JavaScript React Authentication Authorization JWT
toc: 
  - name: Basic Authenticatin process
  - name: NextAuth Overview
  - name: Credentials
    subsections: 
      - name: The login form
      - name: Middleware in Authjs
      - name: Data Access Layer(DAL)
  - name: Session Management
  - name: FAQ
  - name: References
---

## Basic Authenticatin process

- Login button: 
  - Get user information from form.
  - Verify user information.
  - Create session:
    - setup information in JWT, like create signature.
    - create cookie, setup expiration date, etc.
    - put JWT inside cookie

- Every request from browser will send with this session cookie. Server check this JWT and confirm the state of the browser.
  - Also, you can use middleware to handle every request, for example refresh the session cookie.

- Logout button:
  - Simply destroy the session cookie

## Auth.js Overview

Auth.js is an open source auth layer for JavaScript project.
Auth.js was born out of next-auth. And it try to support more frameworks. It keep using the name "NextAuth.js" for Next.js. Here is using "NextAuth" as well.

The latest Authjs version is V5. It has big difference with V4. 

Here are talking about V5. But there are not abundant official document for Authjs V5. 

### 4 authenticate methods
NextAuth provide 4 ways to authenticate users:

- **"OAuth authentication"** (Sign in with Google, GitHub, LinkedIn, etc…)
- **"Magic Links"** (Email Provider like Resend, Sendgrid, Nodemailer etc…)
- **"Credentials"** (Username and Password, Integrating with external APIs, etc…)
- **"WebAuthn"** (Passkeys, etc…)

### Auth.js Framework

NextAuth provide the whole Auth framework structure. Your project will configure your authentication by using this structure. 
How to configure your authentication? 
- `auth.config.ts` and `auth.ts` are the main files you need to configure.
- middleware play important role.

For example signin process: 
- NextAuth frameword provide `Signin` function. 
- To signin your users, make sure you have at least one authentication method setup.
- You can use username and password or other external authentication mechanisms.
- For example, using username and password, we need to setup `auth.ts` use the `Credentials` provider. `authorize()` gives full control over how you handle the `credentials` received from the user. `authorize: (credentials, request) => Awaitable<null | User>;`.

```ts
export const { handlers, signIn, signOut, auth } = NextAuth({
  providers: [
    Credentials({
      // You can specify which fields should be submitted, by adding keys to the `credentials` object.
      // e.g. domain, username, password, 2FA token, etc.
      credentials: {
        email: {},
        password: {},
      },
      authorize: async (credentials) => {
        let user = null
        // logic to salt and hash password
        const pwHash = saltAndHashPassword(credentials.password)
        // logic to verify if the user exists
        user = await getUserFromDb(credentials.email, pwHash)
        if (!user) {
          // No user found, so this is their first attempt to login
          // meaning this is also the place you could do registration
          throw new Error("User not found.")
        }
        // return user object with their profile data
        return user
      },
    }),
  ],
```

- Once a user is logged in, You can get the session object: `const session = await auth()`. Then you can get user information, you can protect the routes. 

### auth.js V5 Setup Steps

The steps roughly like these:

#### Setting up NextAuth.js to your project
- install it: `npm install next-auth@beta`
- generate a secret key for your application. This key is used to encrypt cookies, ensuring the security of user sessions. Like this: `openssl rand -base64 32`
- In your `.env` file, add your generated key to the AUTH_SECRET variable: `AUTH_SECRET=your-secret-key`

#### auth.config.ts file
Create an `auth.config.ts` file at the root of our project that exports an `authConfig` object. 
- you can Add the login pages option in this config file.
- this is used to configure protecting your routes with Next.js Middleware.

Basic logic is that 
1. setup `authorized` callback function inside `authConfig` object. 
2. `export authConfig` object.
3. this `authConfig` object will be use in `middleware.ts`.

> You don't have to use this way to setup auth in middleware. You can use next.js middleware.ts directly.

#### API route
Inside the App folder, create:
```ts
// src/app/api/auth/[...nextauth]/route.ts

export { GET, POST } from '@/auth';
```

#### auth.ts file

`auth.ts` file spreads your `authConfig` object.
```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
import Credentials from 'next-auth/providers/credentials';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [Credentials({})],
});
```

## Credentials

### The login form

The login form for email and password

you create the login route and component where users can input their credentials. for example, create route `/login`.This login page UI should use `form` which is using `authenticate()` Server action to authenticate user.

```js
// app/login/page.tsx
import { authenticate } from '@/app/lib/actions'
 
export default function Page() {
  return (
    <form action={authenticate}>
      <input type="email" name="email" placeholder="Email" required />
      <input type="password" name="password" placeholder="Password" required />
      <button type="submit">Login</button>
    </form>
  )
}
```

Implement `authenticate` Server Action:
```ts
// app/lib/actions.ts
'use server'
import { signIn } from '@/auth'
export async function authenticate(_currentState: unknown, formData: FormData) {
  try {
    await signIn('credentials', formData)
  } catch (error) {
    // ...
  }
}
```

Now, you can add Form Validation implement using `useFormState` (`useActionState`) and `useFormStatus`.

### Middleware in Authjs

There are tow options to use Middelware in Auth.js: 

#### Option 1
You can create a middleware.ts file in your root pages directory with the following contents.
```ts
// middleware.ts
export { auth as middleware } from "@/auth"
```
Then define authorized callback in your auth.ts file. For more details check out the reference docs.
```ts
// auth.ts
import NextAuth from "next-auth"
 
export const { auth, handlers } = NextAuth({
  callbacks: {
    authorized: async ({ auth }) => {
      // Logged in users are authenticated, otherwise redirect to login page
      return !!auth
    },
  },
})
```

#### Option 2
You can also use the auth method as a wrapper if you’d like to implement more logic inside the middleware.
```ts
// middleware.ts
import { auth } from "@/auth"
 
export default auth((req) => {
  if (!req.auth && req.nextUrl.pathname !== "/login") {
    const newUrl = new URL("/login", req.nextUrl.origin)
    return Response.redirect(newUrl)
  }
})
```

#### Using matcher to difine routes
You can also use a regex to match multiple routes or you can negate certain routes in order to protect all remaining routes. 
The following example avoids running the middleware on paths such as the favicon or static images.
```ts
// middleware.ts
export const config = {
  matcher: ["/((?!api|_next/static|_next/image|favicon.ico).*)"],
}
```
Middleware will protect pages as defined by the matcher config export. 

### Data Access Layer(DAL)

- For new next.js projects, It is good to use Data Access Layer(DAL) to consolidate all data access in there.
- **The principle** is that a Server Component function body should only see data that the current user issuing the request is authorized to have access to.
- **The concept** is that building an internal JavaScript library that provides custom data access checks before giving it to the caller. Similar to HTTP endpoints but in the same memory model. 
- Every API should accept the current user and check if the user can see this data before returning it.

From this point, normal security practices for implementing APIs take over.
```ts
// data/auth.tsx

import { cache } from 'react';
import { cookies } from 'next/headers';
 
// Cached helper methods makes it easy to get the same value in many places
// without manually passing it around. This discourages passing it from Server
// Component to Server Component which minimizes risk of passing it to a Client
// Component.
export const getCurrentUser = cache(async () => {
  const token = cookies().get('AUTH_TOKEN');
  const decodedToken = await decryptAndValidate(token);
  // Don't include secret tokens or private information as public fields.
  // Use classes to avoid accidentally passing the whole object to the client.
  return new User(decodedToken.id);
});
```

```ts
// data/user-dto.tsx

import 'server-only';
import { getCurrentUser } from './auth';
 
function canSeeUsername(viewer: User) {
  // Public info for now, but can change
  return true;
}
 
function canSeePhoneNumber(viewer: User, team: string) {
  // Privacy rules
  return viewer.isAdmin || team === viewer.team;
}
 
export async function getProfileDTO(slug: string) {
  // Don't pass values, read back cached values, also solves context and easier to make it lazy
 
  // use a database API that supports safe templating of queries
  const [rows] = await sql`SELECT * FROM user WHERE slug = ${slug}`;
  const userData = rows[0];
 
  const currentUser = await getCurrentUser();
 
  // only return the data relevant for this query and not everything
  // <https://www.w3.org/2001/tag/doc/APIMinimization>
  return {
    username: canSeeUsername(currentUser) ? userData.username : null,
    phonenumber: canSeePhoneNumber(currentUser, userData.team)
      ? userData.phonenumber
      : null,
  };
}
```

These methods should expose objects that are safe to be transferred to the client as is. We like to call these Data Transfer Objects (DTO) to clarify that they're ready to be consumed by the client.

They might only get consumed by Server Components in practice. This creates a layering where security audits can focus primarily on the Data Access Layer while the UI can rapidly iterate. Smaller surface area and less code to cover makes it easier to catch security issues.

```ts
import {getProfile} from '../../data/user'
export async function Page({ params: { slug } }) {
  // This page can now safely pass around this profile knowing
  // that it shouldn't contain anything sensitive.
  const profile = await getProfile(slug);
  ...
}
```
Secret keys can be stored in environment variables but only the data access layer should access process.env in this approach.

### Protecting Server Actions
Implement checks within Server Actions to determine user permissions, such as restricting certain actions to admin users.

```ts
// app/lib/actions.ts
'use server'

export async function serverAction() {
  const session = await getSession()
  const userRole = session?.user?.role
 
  // Check if user is authorized to perform the action
  if (userRole !== 'admin') {
    throw new Error('Unauthorized access: User does not have admin privileges.')
  }
 
  // Proceed with the action for authorized users
  // ... implementation of the action
}
```

### Protecting Route Handlers

You can do this:
```ts
// app/api/route.ts
export async function GET() {
  // User authentication and role verification
  const session = await getSession()
 
  // Check if the user is authenticated
  if (!session) {
    return new Response(null, { status: 401 }) // User is not authenticated
  }
 
  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    return new Response(null, { status: 403 }) // User is authenticated but does not have the right permissions
  }
 
  // Data fetching for authorized users
}
```

### Authorization Using Server Components

Server Components can direct access to back-end resources. So need do Authorization.
A common practice is to conditionally render UI elements based on the user's role. 
Like this:
```ts
// app/dashboard/page.tsx

export default async function Dashboard() {
  const session = await getSession()
  const userRole = session?.user?.role // Assuming 'role' is part of the session object
 
  if (userRole === 'admin') {
    return <AdminDashboard /> // Component for admin users
  } else if (userRole === 'user') {
    return <UserDashboard /> // Component for regular users
  } else {
    return <AccessDenied /> // Component shown for unauthorized access
  }
}
```

## OAuth

## Magic Links

## WebAuthn (Passkeys)

## Session Management

(When you use NextAuth, it provide all the function. If you gonna to do it by yourself, then fellowing can give you some hint.)
Session management involves tracking and managing a user's interaction with the application over time, ensuring that their authenticated state is preserved across different parts of the application.

This prevents the need for repeated logins, enhancing both security and user convenience. There are two primary methods used for session management: 
- cookie-based (storing session data on the User Browser, data should be encrypted)
- database sessions（storing session data on the server）

### Implement cookie-based Session management

Setting a cookie on the server action:
```ts
// app/actions.ts
'use server' 
import { cookies } from 'next/headers'

export async function handleLogin(sessionData) {
  const encryptedSessionData = encrypt(sessionData) // Encrypt your session data using JWT
  cookies().set('session', encryptedSessionData, {
    httpOnly: true,
    secure: process.env.NODE_ENV === 'production',
    maxAge: 60 * 60 * 24 * 7, // One week
    path: '/',
  })
  // Redirect or handle the response after setting the cookie
}
```

Accessing the session data stored in the cookie in a server component:
```ts
// app/page.tsx
import { cookies } from 'next/headers'
 
export async function getSessionData(req) {
  const encryptedSessionData = cookies().get('session')?.value
  return encryptedSessionData ? JSON.parse(decrypt(encryptedSessionData)) : null
}
```

## FAQ

- How NextAuth.js implement OIDC authentication?
- What `signIn()` do in NextAuth.js?
- How to implement Data Access Layer(DAL)

### Using middleware to protect routes when not using Authjs.

Once a user is authenticated, you'll need to think about What user can do:
- visiting certain routes 
- perform operations such as mutating data with Server Actions 
- calling Route Handlers

Middleware in Next.js helps you control who can access different parts of your website.
Here's how to implement Middleware for authentication in Next.js:

- Setting Up Middleware:
  - Create a middleware.ts or .js file in your project's root directory.
  - Include logic to authorize user access, such as checking for authentication tokens.
- Defining Protected Routes:
  - Not all routes require authorization. Use the matcher option in your Middleware to specify any routes that do not require authorization checks.
- Middleware Logic:
  - Write logic to verify if a user is authenticated. Check user roles or permissions for route authorization.
- Handling Unauthorized Access:
  - Redirect unauthorized users to a login or error page as appropriate.

Middleware example:
```ts
//middleware.ts
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  const currentUser = request.cookies.get('currentUser')?.value
 
  if (currentUser && !request.nextUrl.pathname.startsWith('/dashboard')) {
    return Response.redirect(new URL('/dashboard', request.url))
  }
 
  if (!currentUser && !request.nextUrl.pathname.startsWith('/login')) {
    return Response.redirect(new URL('/login', request.url))
  }
}
 
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)'],
}
```

## References

- next-auth@4.x.y on [NextAuth.js.org](https://next-auth.js.org)
- next-auth@5.0.0-beta on [authjs.dev](https://authjs.dev/reference/next-auth).
- [Blog: Security in Next.js](https://nextjs.org/blog/security-nextjs-server-components-actions)
