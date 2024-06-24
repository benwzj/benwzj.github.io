---
layout: post
title: Implement Auth in Next.js
date: 2024-06-13
category: React
tags: Next.js JavaScript React Authentication Authorization
toc: 
  - name: The Basic Authenticatin process
  - name: NextAuth.js
  - name: Implement Authentication
  - name: Implement Authorization
  - name: Session Management
  - name: 
---

## The Basic Authenticatin process

- Login button: 
  - get user information from form.
  - verify user information.
  - create session:
    - setup information in JWT, like create signature.
    - create cookie, setup expiration date, etc.
    - put JWT inside cookie

- every request from browser will send with this session cookie. server check this JWT and confirm the state of the browser.
  - also, you can use middleware to handle every request, for example refresh the session cookie.

- Logout button:
  - simply destroy the session cookie

## NextAuth.js

NextAuth.js is an open source auth layer for Next.js project.
Auth.js was born out of next-auth. And it try to support more frameworks. It keep using the name "NextAuth.js" for Next.js. 

There are 4 ways to authenticate users with Auth.js:

- OAuth authentication (Sign in with Google, GitHub, LinkedIn, etc…)
- Magic Links (Email Provider like Resend, Sendgrid, Nodemailer etc…)
- Credentials (Username and Password, Integrating with external APIs, etc…)
- WebAuthn (Passkeys, etc…)

## Implement Authentication

The step roughly like this:

### Setting up NextAuth.js to your project
- install it: `npm install next-auth@beta`
- generate a secret key for your application. This key is used to encrypt cookies, ensuring the security of user sessions. Like this: `openssl rand -base64 32`
- In your `.env` file, add your generated key to the AUTH_SECRET variable: `AUTH_SECRET=your-secret-key`

### auth.config.ts file
Create an `auth.config.ts` file at the root of our project that exports an `authConfig` object. 
- you can Add the login pages option in this config file.
- configure Protecting your routes with Next.js Middleware.

### auth.ts file
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

### The login form for email and password

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

## Implement Authorization

Once a user is authenticated, you'll need to think about What user can do:
- visiting certain routes 
- perform operations such as mutating data with Server Actions 
- calling Route Handlers

### Protecting Routes with Middleware
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

## Session Management

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

## References

- [NextAuth.js Doc](https://next-auth.js.org)
- [Auth.js Doc](https://authjs.dev/reference/next-auth).
- [Blog: Security in Next.js](https://nextjs.org/blog/security-nextjs-server-components-actions)
