---
layout: post
title: Shopify Dev Overview
date: 2024-05-30
category: Website
tags: Shopify E-Commerce
---

There are three parts that developers can build on: Apps, Themes, Headless(custom storefront).

Run through this blog, you can get to know some concepts and terminologies of Shopify develpment.

## Apps

### What is Shopify App?

- Shopify itself meets about 80% of global merchant needs. For everything else, Shopify merchants turn to apps.
- Shopify offers APIs, toolings and integration point to App. 
- App Connects with Shopify APIs to extend store features. 
- Merchant installs and uses App in their store.
- Shopify Apps can appear in and add functionality to nearly every area of the Shopify platform. 
- A single app can add functionality to multiple areas of the platform. 
- Depending on the tool that you're using, Shopify might host your code, or you might have to arrange for hosting yourself.
- You can Surface App's functionality in Shopify admin, Checkout, Online Store, Shopify Flow, Point of Sale, etc.

#### Shopify admin

The Shopify admin is the primary interface for merchants to manage their stores, and core Shopify objects such as products, orders, and customers. 
The Shopify admin is also the place where merchants can manage their apps. Developers can surface their app's functionality in the Shopify admin using the following technologies:
- Embedded app pages directly inside of the Shopify admin.
  - Shopify App Bridge (A JavaScript library)
  - Polaris design system
  - Shopify app templates
- Custom data (more data models except for basic, like products, collections, and orders)
  - Metafields
  - Metaobjects
- App extensions

#### Surface App's functionality in Checkout
Merchants use Shopify checkout to accept orders and receive payments wherever they sell online. You can add functionality to Shopify checkout by building an app.
using the following technologies:
- Checkout UI extensions
- Shopify Functions
- Post-purchase checkout extensions

### Online Store
The online store is a storefront that merchants use to sell their products. You can add functionality to the online store using the following technologies:
- Theme app extensions
- Web pixels
- Storefront API

#### Shopify Flow
Shopify Flow is an app and platform that lets merchants customize their store through automation. You can integrate your app with Shopify Flow through custom triggers and actions. Triggers enable your app to start a Flow workflow. Actions enable Flow to call your app or service in a workflow to do work.

#### Shopify Point of Sale (POS)
Shopify Point of Sale (POS) is a point of sale app that merchants can use to sell products in person. Merchants can create a cart for each customer, customize the cart in several ways, and then accept payment with a wide range of payment methods. You can surface your app's functionality in Shopify POS through standardized APIs and extensions.

### Build Shopify App

- You need a Partner account and a development store.
- You need to use Shopify CLI. like this: `shopify app init`
- Shopify recommend use Remix to build Shopify App. But you can still other framework.

## FQA

- What is storefront

### What is Point of Sale (POS)

The point of sale (POS) is the time and place at which a retail transaction is completed. At the point of sale, the merchant calculates the amount owed by the customer, indicates that amount, prepare an invoice for the customer (which may be a cash register printout), and indicates the options for the customer to make payment.

### What is Shopify Point of Sale (POS)

Shopify POS is a point of sale **app** that you can use to sell your products in person. 
Process orders, accept payments, produce receipts, and control it all from your mobile device. Shopify online store automatically synchronizes with Shopify POS, and you manage your entire business from one dashboard. Inventory, product, and payment updates that you make in your Shopify admin will instantly take effect in Shopify POS.

### What is App extension

- An app extension enables you to add your app's functionality to Shopify user interfaces.
- An app extension isn’t an app. It's a mechanism that lets an app add features to certain defined parts of several Shopify user interfaces
- **Without** an app extension, users interact directly with your app. Your app relays information to Shopify that gets surfaced back to the users through your app.
- **With** an app extension, users interact with Shopify. Shopify relays information to your app that gets surfaced back to the users through your app extension in Shopify.


## References

- [shopify.dev](https://shopify.dev)
