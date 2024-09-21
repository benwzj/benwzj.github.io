---
layout: post
title: Next.js Shopify Ecommerce Application
date: 2024-05-15
category: React
tags: GraphQL Next.js Shopify Ecommerce
toc:
  - name: Setup Shopify
  - name: GraphQL APIs
  - name: Implement Shopping Cart
  - name: Authentication without Next-Aut
  - name: Admin Function
  - name: FAQ
  - name: References
---

- tailwindcss
- typescript
- @headlessui/react


## Setup Shopify

Build online storefront in Shopify. Use Next.js Commerce as your headless Shopify theme. 

### Step:
- Configure Shopify for use as a headless CMS.
  - install the Shopify Headless theme. 
  - install Headless App.
    - retrieve 'public access token' for Store Front API. This Ecommerce App will use Store Front API to interact with Shopify CMS.
- You can deploy this app on Vercel Cloud.
  - Configure environment variables in Vercel.
- You can deploy on your own server. Remember to configure the `.env` file.

Also, you need to add 'page', 'menu' and 'Collections' in Shopify headless store.
- Collections: `hidden-homepage-carousel`, `hidden-homepage-featured-items`;
- Page: You can create page like: `FAQ`, `Contact`, etc.
- Menu: `next-js-frontend-header-menu`, `next-js-frontend-footer-menu`

## GraphQL APIs

### Overview
- Implement GraphQL API using TypeScript.
- All Shopify API defined in `lib/shopify`.
- `shopifyFetch()` is the function which fetch data from Shopify CMS throught GraphQL.
  - It is the main entrance.
  - It is generic function. 
- For example: `getCart(cartId)` -> `shopifyFetch({query: getCartQuery, variables: { cartId }})`, then the Response is Items detail on Cart.

### `shopifyFetch()` 
- All operations to shopify is go through `shopifyFetch()`.
- `shopifyFetch()` is a generic function. The type `T` is used for different Variables structure which used for defferent GraghQL Request.
- Use `fetch` function to connect shopify GraphQL endpoint.
  - the header contain `X-Shopify-Storefront-Access-Token`.
  - the body contain the `query` string.


## Implement Shopping Cart

- Defined in `components/cart`
- Use cookie to store Cart ID. Cart detail information store in Shopify.
- Use GraphQL API to manage cart in Shopify.
- Display Cart in Modal.

### Steps:
- Shopping `<Cart>` component is rendered at the very beginning inside `<Suspense>` within `navbar`.
- Cart component read cookie to get Cart ID.
- Fetch shopping cart items from Shopify DMS by GraphQL API according to Cart ID.
- Pass cart items to `<CartModal>`.
- `<CartModal>` is the main component to implement Shopping Cart. 

### CartModal Component
- It is Modal.
- It is client component: `'use client';`
- The effect of drawing in and out is using `<Transition>` and `<Dialog>` which come from 3rd lib `"headlessui"`.

### Server Actions
They defined in `components/cart/actions`.
- addItem
- removeItem
- updateItemQuantity

### getCart(cartId) detail
`cartId` is come from cookie, if no, create one in shopify and retrieve a cartId and store in cookie.
`getCart(cartId)`use `shopifyFetch()` to send GraphQL to shopify.
- query: `getCartQuery`,
- variables: `{ cartId }`,

### addToCart() detail
`addToCart()` use `shopifyFetch()` to send GraphQL to shopify.
- `variables`: `cartId`, `line`. line is array of `{merchandiseId: string; quantity: number;}`
- `query`: `addToCartMutation`

## Authentication without Next-Auth

Reference [nextjs.org/docs](https://nextjs.org/docs/app/building-your-application/authentication).

### Sign Up process

- Create Sign Up Dialog to collect user information, including email and password etc.
- Using `useFormState` submit to server action `signup`.
- `signup` use `Zod` to validate formData.
- server action `signup` use Shopify Storefront GraphQL API: `mutation customerCreate` to create user. This user will be stored as custmer in Shopify server.

### Login process

- Create Login Dialog to collect user email and password.
- Using `useFormState` submit to server action `authenticate`.
- `authenticate` use `Zod` to validate formData.
- `authenticate` create `customerAccessToken` from Shopify server according to username and password. This process is using Shopify Storefront GraphQL API: `mutation customerAccessTokenCreate`
- `authenticate` use Shopify Storefront GraphQL API: `query customer` and `customerAccessToken` to fetch user information.
- Implement cookie-based Session management.

### Session management
- create a seesion.ts file 
  - create `encrypt`, `decrypt` functions which are using `jose` to implement.
  - create `createSession` function: set cookie().
  - create `getSession()` function: get cookie().
  - create `updateSession()` funciton: use in middelware.ts for extend cookie expire.
  - create `deleteSession` function: delete cookie().
- When user login or signup successfully, you can `createSession`. When logout, `deleteSession`.
- use `updateSession()` funciton in `middelware.ts`.
- You can use `getSession()` function to get state of login anywhere in your project.

#### How to get to know the state of authentication?
Read Cookies can get the state of authentication, but the cookiet can be read only in Server action, middleware, Route handler. For example, displaying login logout signou logo in main menu is deponding on authenticaiton state. How can i don this?

Cookies can be read anywhere, Not just server actions middleware or route handler.

## Admin Function

## FAQ
- Where do it store items information in Cart? 
Cookies store CartID. All cart information is store at Shopify CMS.
- How to interact with Shopify? 
Use Storefront GraphQL API.
- What code style like when coding in Next.js? 
  - How do folders, files distribute?
  - How to make Components? 

## References
- Basa on [Next.js Commerce template](https://github.com/vercel/commerce).
- Use [GraphQL Storefront API](https://shopify.dev/docs/api/storefront).
