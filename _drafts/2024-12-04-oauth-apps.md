---
layout: post
title: What is OAuth Apps
date: 2024-12-04
category: Auth
tags: Authentication github Authorization
---

1. GitHub support OAuth Apps and GitHub Apps.
2. Shopify support Apps.
3. Google support Apps.


Let's clear what are this apps.

## What is OAuth Apps

### Definition
OAuth apps are applications that use the OAuth protocol to allow users to grant third-party access to their data without sharing their password. 

### Example 1 

Vercel provide Server platform for developer to deploy their application which code can be store in GitHub. If you want to allow Vercel to access GitHub to deploy your application, you need to install an OAuth app in your Repo. After the OAuth installation, Vercel can access that Reop.

### Example 2

When You build an application which support Google SignIn, you need to ceate a project which use **Credentials** APIs and services. From there you need to setup callback URIs and you will get an "OAuth 2.0 Client IDs" and "Client secret" for you application. 
You can say this google project work as OAuth App. This OAuth App is using Google APIs to ask users login and access their profiles.

Sometime the OAuth App can be very complicated and you can buy it from software markey.

### OAuth Concept
OAuth, or Open Authorization, is a standard that allows users to authorize apps to access their data and resources without sharing their login credentials. OAuth uses access tokens to grant temporary access to third parties. These tokens contain information about the user and the resource, as well as specific rules for data sharing. 

### OAuth Benefits
OAuth allows users to: 
- Share specific data with an application while keeping their usernames, passwords, and other information private 
- Use one-click logins to identify themselves to a web service without having to enter their username and password every time 


## GitHub Apps and OAuth apps

GitHub Apps are tools that extend GitHub's functionality. GitHub Apps can do things on GitHub like open issues, comment on pull requests, and manage projects. They can also do things outside of GitHub based on events that happen on GitHub. For example, a GitHub App can post on Slack when an issue is opened on GitHub.

GitHub also supports OAuth apps. In general, GitHub Apps are preferred over OAuth apps. GitHub Apps use fine-grained permissions, give the user more control over which repositories the app can access, and use short-lived tokens. These properties can harden the security of the app by limiting the damage that could be done if the app's credentials were leaked.


