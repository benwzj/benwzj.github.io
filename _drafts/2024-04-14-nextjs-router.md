---
layout: post
title: Next.js App Router Introduction
date: 2024-04-14
category: React
tags: React Next.js Router
---

## Routing Fundamentals

The skeleton of every application is routing. 

## The app Router
In version 13, Next.js introduced a new App Router built on React Server Components, which supports shared layouts, nested routing, loading states, error handling, and more.

The App Router works in a new directory named `app`. The `app` directory works alongside the `pages` directory to allow for incremental adoption. This allows you to opt some routes of your application into the new behavior while keeping other routes in the pages directory for previous behavior. 

