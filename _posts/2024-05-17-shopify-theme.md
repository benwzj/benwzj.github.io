---
layout: post
title: Shopify Theme
date: 2024-05-17
category: Website
tags: Shopify Liquid Theme
---


A Shopify theme determines the way that a Shopify online store looks, feels, and functions for merchants and their customers.
Different themes have different styles and layouts, and offer a different experience for your customers. For example, if you're selling spa products, then you might want your online store to feel relaxed and luxurious. If you're selling electronics, then you might want your online store to look energetic and sleek.

Shopify themes are built using Shopify's theme templating language, Liquid, along with HTML, CSS, JavaScript, and JSON. Using these languages, developers can create any look and feel that their clients want. 

> [Dawn](https://github.com/Shopify/dawn) is official, free, default theme. It show What a Theme exactly to be.
{: .block-warning}

## Theme architecture

Theme code is organized with a standard directory structure of files specific to Shopify themes, as well as supporting assets such as images, stylesheets, and scripts.

### Theme files
Theme files fall into the following general categories:

- Markup and features - These files control the layout and functionality of a theme. They use **Liquid** to generate the HTML markup that makes up the pages of the merchant's online store.
- Supporting assets - These files are assets, scripts, or locale files that are either called or consumed by other files in the theme.
- Config files - These files use **JSON** to store configuration data that can be customized by merchants using the theme editor.

### Themes directory structure
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
- The `layout` directory contains the layout files for a theme, through which template files are rendered. A `theme.liquid` file must exist in this folder for the theme to be uploaded to Shopify. Layouts are **Liquid files** that allow you to include content, that should be repeated on multiple page types, in a single location. For example, layouts are a good place to include any content you might want in your `<head>` element, as well as `headers` and `footers`.
  - In JSON templates, the layout that's used to render a page is specified using the layout attribute.
  - In Liquid templates, the layout that's used to render a page is specified using the layout Liquid tag.
- The `locales` directory contains the locale files for a theme, which are used to provide translated content.
- The `sections` directory contains a theme’s sections and section groups. Sections are Liquid files, while Section groups are JSON containers.
- The `snippets` directory contains Liquid files that host smaller reusable snippets of code.
- The `templates` directory contains a theme’s template files, which control what's rendered on each type of page.
- The `templates/customers` directory contains the template files for customer-centric pages like the login and account overview pages.

## Template

Templates control what's rendered on each type of page in a theme.
Template files are located in the templates directory of the theme.

### File types

There are two different file types you can use for a theme template: JSON and Liquid. 
#### JSON	file
JSON templates are data files with the `.json` file extension. These templates let you easily populate your template with content from sections. Sections can be added, removed, or rearranged by merchants using the theme editor.
If you're using a JSON template, then any HTML or Liquid code needs to be included in a section that's referenced by the template.
If you want to use sections in a template, then you should use a JSON template.
JSON templates provide more flexibility for merchants to add, remove, and reorder sections, including app sections.

#### Liquid	file
Liquid templates are Liquid markup files, with the `.liquid` file extension. You can add Liquid and HTML directly to Liquid templates.

### Template types

Each available template type represents a type of content in a merchant's online store.
For example, to render a product page, you need at least one template of type `product`.

Template types can be: 
404	
article	
blog.
cart
collection
customers/account
etc.

## FQA
- How Theme work in Shopify


## References

- [Theme architecture](https://shopify.dev/docs/themes/architecture)