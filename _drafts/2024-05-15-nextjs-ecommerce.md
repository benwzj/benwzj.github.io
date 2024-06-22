---
layout: post
title: Next.js Ecommerce
date: 2024-05-15
category: React
tags: GraphQL Next.js Shopify
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

## Implement Shopping Cart

- Shopping Cart component is rendered at the very beginning inside `<Suspense>` within `navbar`.
- Shopping Cart will read cookie and retrieve items.
- `<CartModal>` is the main component to implement Shopping Cart.
- The effect of drawing in and out is using `<Transition>` and `<Dialog>` which come from third lib "headlessui".


### FAQ
- Where do it store items information in Cart?  It looks like in Cookie
- What items information store in Cookie?
- How to interact with Shopify?
- What code style like when coding in Next.js? 
  - How do folders, files distribute?
  - How to make Components? 



## Add Login function


## References
- Basa on [Next.js Commerce template](https://github.com/vercel/commerce).
- Use [GraphQL Storefront API](https://shopify.dev/docs/api/storefront).
