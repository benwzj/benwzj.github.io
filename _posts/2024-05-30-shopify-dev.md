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

#### Checkout
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

#### Shopify POS
Shopify Point of Sale (POS) is a point of sale app that merchants can use to sell products in person. Merchants can create a cart for each customer, customize the cart in several ways, and then accept payment with a wide range of payment methods. You can surface your app's functionality in Shopify POS through standardized APIs and extensions.

### Build Shopify App

- You need a Partner account and a development store.
- You need to use Shopify CLI. like this: `shopify app init`.
- Shopify recommend use Remix to build Shopify App. But you can still other framework.

## Theme

### What is Shopify Theme

- A Shopify theme determines the way that a Shopify online store looks, feels, and functions for merchants and their customers.
- Different themes have different styles and layouts, and offer a different experience for your customers. For example, if you're selling spa products, then you might want your online store to feel relaxed and luxurious. If you're selling electronics, then you might want your online store to look energetic and sleek.
- Shopify themes are built using Shopify's theme templating language, Liquid, along with **HTML, CSS, JavaScript, and JSON**. Using these languages, developers can create any look and feel that their clients want. 
- [Dawn](https://github.com/Shopify/dawn) is official, free, default theme. It show What a Theme exactly to be.

### Theme architecture

Theme code is organized with a standard directory structure of files specific to Shopify themes, as well as supporting assets such as images, stylesheets, and scripts.

#### Theme file categories

Theme files fall into the following general categories:
- **Markup and features** - These files control the layout and functionality of a theme. They use **Liquid** to generate the HTML markup that makes up the pages of the merchant's online store.
- **Supporting assets** - These files are assets, scripts, or locale files that are either called or consumed by other files in the theme.
- **Config files** - These files use **JSON** to store configuration data that can be customized by merchants using the theme editor.

#### Themes directory structure

Themes must use the following directory structure:
```
.
├── assets
├── config
├── layout
├── locales
├── sections
├── snippets
└── templates
    └── customers
```

- The `assets` directory, including image, CSS, and JavaScript files.
- The `config` directory contains the config files for a theme. 
- The `layout` directory contains the layout files for a theme, through which template files are rendered. A `theme.liquid` file must exist in this folder for the theme to be uploaded to Shopify. 
- The `locales` directory contains the locale files for a theme, which are used to provide translated content.
- The `sections` directory contains a theme’s sections and section groups. Sections are Liquid files, while Section groups are JSON containers.
- The `snippets` directory contains Liquid files that host smaller reusable snippets of code.
- The `templates` directory contains a theme’s template files, which control what's rendered on each type of page.
- The `templates/customers` directory contains the template files for customer-centric pages like the login and account overview pages.

### Theme Layout

- Layouts are the base of the theme, through which all templates are rendered.
- Layouts are Liquid files.
- Layout files are located in the `layout` directory of the theme.
- Layout files allow you to include content, that should be **repeated** on multiple page types, in a single location. For example, layouts are a good place to include any content you might want in your `<head>` element, as well as headers and footers.
- There are the two layout types: General, Checkout.
- Most layout files also contain the following Schema objects: `content_for_header`, `content_for_layout`.

### Theme Template

- Theme Template files located in `templates` directory.
- Templates control what's rendered on each type of page in a theme.
- Each page type in an online store has an associated template type. For example, to render a product page, the theme needs at least one template of type `product`.
- There are two different file types you can use for a theme template: JSON and Liquid. 
- Template types can be: 
`404`,
`article`,
`blog`,
`cart`,
`collection`,
`customers/account`,
etc.

### Sections

- Section files are located in the `sections` directory of the theme.
- Sections are Liquid files that allow you to create reusable modules of content that can be customized by **merchants**.
- For example, you can create an Image with text section that displays an image and text side-by-side with options for merchants to choose the image, set the text, and select the display order.

### Blocks

- Blocks are reusable modules for structuring content within sections. 
- Blocks can represent a variety of content types such as text, images, products, collections and videos. They can be added, removed, and reordered within a section, providing merchants with a high degree of flexibility and customization in the theme editor.

## Headless

### What is Headless
- Headless means total customized storefront which give you maximum flexibility. 
- You can build Headless with Shopify’s Storefront APIs and Customer Account APIs.
- Hydrogen is Shopify official headless framework. You can alwo bring your own stack.
- Oxygen is Shopify’s global serverless hosting platform, built for deploying Hydrogen storefronts at the edge.
- custom storefron, not just website, it can be native mobile app, PWA, video livestreams, IoT, or just add a buy button on an existing website.


## FQA

### What is Storefront
Storefront is the interface to customers from which you can sell things.
There are two way to build your Storefront: Using Theme or Headless.

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

### How Theme work in Shopify

### What is headless Shopify theme
A theme determines the way that a Shopify online store looks, feels, and functions for merchants and their customers.
Shopify themes are built using Shopify's theme templating language, Liquid, along with HTML, CSS, JavaScript, and JSON. Using these languages, developers can create any look and feel that their clients want. 
Headless Theme is used for Headless storefront.

## References

- [shopify.dev](https://shopify.dev)