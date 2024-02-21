---
layout: post
title: ExpressJS vs. NestJS
date: 2023-12-01
category: JavaScript
tags: TypeScript JavaScript NodeJS Framework
---

Both of them are Server side application frameworks for NodeJS.

## ExpressJS

Express is the most popular Node web framework, and is the underlying library for a number of other popular Node web frameworks (including NestJS). 

According to ExpressJS official website, ExpressJS is Fast, unopinionated, minimalist web framework for Node.js.
The strengths of Express is its strong community and the number of available plugins.

It provides mechanisms to:

- Write handlers for requests with different HTTP verbs at different URL paths (routes).
- Integrate with "view" rendering engines in order to generate responses by inserting data into templates.
- Set common web application settings like the port to use for connecting, and the location of templates that are used for rendering the response.
- Add additional request processing "middleware" at any point within the request handling pipeline.

While Express itself is fairly minimalist, developers have created compatible **middleware** packages to address almost any web development problem. There are libraries to work with cookies, sessions, user logins, URL parameters, POST data, security headers, and many more. You can find a list of middleware packages maintained by the Express team at [Express Middleware](https://expressjs.com/en/resources/middleware.html) (along with a list of some popular 3rd party packages).

> Because of flexibility, Express need more knowledge, understanding to work with.

## NestJS

Nest is a progressive framework. It is built with TypeScript and combines elements of OOP (Object Oriented Programming), FP (Functional Programming), and FRP (Functional Reactive Programming).

Under the hood, Nest makes use of ExpressJS.
The architecture of NestJS is heavily inspired by Angular.
NestJS is opinionated on purpose.

## ExpressJS vs. NestJS

The biggest difference between these frameworks is that NestJS is opinionated, and ExpressJS is not. 

It means Express gives developers the freedom to make multiple possibilities and implement code as per the need, as it doesn’t have a set of pre-defined rules to follow. Such a flexibility is appreciated by many developers, and it’s beneficial for smaller flexible teams, but once team size and app’s complexity grows, the lack of structure becomes a problem.