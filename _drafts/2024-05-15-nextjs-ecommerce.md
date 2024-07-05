---
layout: post
title: Next.js Ecommerce
date: 2024-05-15
category: React
tags: GraphQL Next.js Shopify
toc:
  - name: Setup Shopify
  - name: GraphQL APIs
  - name: Implement Shopping Cart
  - name: Add Login function
  - name: Admin Function
  - name: References
---

- tailwindcss
- typescript
- @headlessui/react


## Setup Shopify

Build online storefront in Shopify. Use Next.js Commerce as your headless Shopify theme. 
To use Next.js Commerce as your headless Shopify theme, you need to install the Shopify Headless theme. 

- you can add 'page' in Shopify.
- you can add 'menu' in Shopify.
- Shopify support fetch Collections API, fetch Menu API.

### Setup Step:
- Configure Shopify for use as a headless CMS.
- Deploy your headless storefront on Vercel.
- Configure environment variables in Vercel.

## GraphQL APIs

- Implemen GraphQL API using TypeScript.
- All Shopify API defined in `lib/shopify`.
- `shopifyFetch()` is the function which fetch data from Shopify CMS throught GraphQL.
  - It is generic function. 
- For example: `getCart(cartId)` -> `shopifyFetch({query: getCartQuery, variables: { cartId }})`, then the Response is Items detail on Cart.

## Implement Shopping Cart

- Defined in `components/cart`
- Use cookie to store Cart ID. Cart detail information store in Shopify.
- Use GraphQL API to manage cart in Shopify.
- Display Cart in Modal.

### Steps:
- Shopping `<Cart>` component is rendered at the very beginning inside `<Suspense>` within `navbar`.
- Cart component read cookie to get Cart ID.
- Retrieve shopping cart items from Shopify DMS by GraphQL API according to Cart ID.
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


### Questions for Cart
- Where do it store items information in Cart?  It looks like in Cookie
- What items information store in Cookie?
- How to interact with Shopify?
- What code style like when coding in Next.js? 
  - How do folders, files distribute?
  - How to make Components? 


## Add Login function


## Admin Function

## References
- Basa on [Next.js Commerce template](https://github.com/vercel/commerce).
- Use [GraphQL Storefront API](https://shopify.dev/docs/api/storefront).
