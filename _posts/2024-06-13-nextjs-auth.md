---
layout: post
title: Implement Auth in Next.js
date: 2024-06-13
category: React
tags: Next.js JavaScript React Authentication Authorization
toc: 
  - name: Authentication
  - name: Authorization
  - name: Session Management
  - name: Auth.js
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


There parts will be talked about:  
Authentication, Authorization, Session Management.

## Authentication

There are many authentication solutions are compatible with Next.js. Like, Auth0, Clerk, Kinde etc.
You can add these solutions to your application. 
They all support Modern authentication strategies, like OAuth/OpenID Connect (OIDC), Credentials-based login (Email + Password), Passwordless/Token-based authentication.

I use NextAuth.js in this blog. It is open source and it includes built-in support for 4 high-level authentication types - OAuth / OICD, Email magic-links, WebAuthn / Passkeys and username/password credentials.

How NextAuth.js Work? 
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

## Authorization

Once a user is authenticated, you'll need to think about What user can do:
- visiting certain routes 
- perform operations such as mutating data with Server Actions 
- calling Route Handlers

### Protecting Routes with Middleware
Middleware in Next.js helps you control who can access different parts of your website.

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

With authentication solutions such as NextAuth.js, session management becomes more efficient, using either cookies or database storage. 

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

## Auth.js (NextAuth.js)

Auth.js was born out of next-auth. And it try to support more frameworks. It keep using the name "NextAuth.js" for Next.js. 

There are 4 ways to authenticate users with Auth.js:

- OAuth authentication (Sign in with Google, GitHub, LinkedIn, etc…)
- Magic Links (Email Provider like Resend, Sendgrid, Nodemailer etc…)
- Credentials (Username and Password, Integrating with external APIs, etc…)
- WebAuthn (Passkeys, etc…)




## FAQ

- How NextAuth.js implement OIDC authentication?
- What `signIn()` do in NextAuth.js?

## References

- [NextAuth.js Doc](https://next-auth.js.org)
- [Auth.js Doc](https://authjs.dev/reference/next-auth).
