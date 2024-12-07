---
layout: post
title: OAuth Flow Details
date: 2024-12-07
category: Auth
tags: Authentication github Authorization OAuth
---

OAuth is a standard. There are many company implement OAuth according to this Standard. The implementation should be very similar.

First of all, you need to create OAuth App in Authorization Server.

In OAuth Flow, usually there are some Roles: 
- User
- Browser
- Website App
- The Authorization Server
- The API

There are different OAuth Flow for defferent app types:
- Single-page app (SPA)
- Web app
- Web API
- Mobile and native apps
- Service, daemon, script

## Web app flow (github implementation)

The web application flow to authorize users for your app is:
1. Users are redirected to request their GitHub identity
2. Users are redirected back to your site by GitHub
3. Your app accesses the API with the user's access token

### 1. Request a user's GitHub identity
```
GET https://github.com/login/oauth/authorize
```
This endpoint takes the following input parameters.
- `client_id`	string	Required
- `redirect_uri`	string	Strongly recommended
- `login`	string	Optional
- `scope`	string	Context dependent
- `state`	string	Strongly recommended

### 2. Users are redirected back to your site by GitHub
If the user accepts your request, GitHub redirects back to your site with a temporary code(authorization code) in a code parameter as well as the state you provided in the previous step in a state parameter. 

Exchange this code for an access token:
```
POST https://github.com/login/oauth/access_toke
```
This endpoint takes the following input parameters.
- `client_id`	string	Required
- `client_secret`	string	Required
- `code`	string	Required
- `redirect_uri`	string	Strongly recommended

By default, the response takes the following form:
```
access_token=gho_16C7e42F292c6912E7710c838347Ae178B4a&scope=repo%2Cgist&token_type=bearer
```

### 3. Use the access token to access the API
The access token allows you to make requests to the API on a behalf of a user.
```
Authorization: Bearer OAUTH-TOKEN
GET https://api.github.com/user
```
For example, in curl you can set the Authorization header like this:
```
curl -H "Authorization: Bearer OAUTH-TOKEN" https://api.github.com/user
```
Every time you receive an access token, you should use the token to revalidate the user's identity. A user can change which account they are signed into when you send them to authorize your app, 

{% include figure.html path="assets/img/oauth_web_server_flow.png" class="img-fluid rounded z-depth-1" %}

Another one:

{% include figure.html path="assets/img/oauth-flow.webp" class="img-fluid rounded z-depth-1" %}

### About Redirect_URI 

Redirect_RUI need be defined in the Authorization Server. 
And When User is using GET and POST to access Authorization Server, it can use the `redirect_uri` parameter. 
- If left out, GitHub will redirect users to the callback URL configured in the OAuth app settings. 
- If provided, the redirect URL's host (excluding sub-domains) and port must exactly match the callback URL. The redirect URL's path must reference a subdirectory of the callback URL.

#### Loopback redirect urls
The optional `redirect_uri` parameter can also be used for loopback URLs, which is useful for native applications running on a desktop computer. If the application specifies a loopback URL and a port, then after authorizing the application users will be redirected to the provided URL and port. The `redirect_uri` does not need to match the port specified in the callback URL for the app.

You can use `http://127.0.0.1:1234/path` as `redirect_uri`.

> Note that OAuth RFC recommends not to use `localhost`, but instead to use loopback literal `127.0.0.1` or `IPv6 ::1`.


