---
layout: post
title: All about Authentication
date: 2024-06-15
category: Auth
tags: Authentication Authorization JWT OAuth OpenID OIDC
---

## Term Overview

### OAuth vs OpenID vs OIDC
- OpenID is based on a simple idea: a user authenticates with an identity provider (IDP), who then provides the user with a **unique identifier** (called an OpenID). This identifier can then be used to authenticate the user with any website that supports OpenID.
- OAuth(Open Authorization), Originally designed to for applications to get access to APIs. grants access to Other resources via **Access Tokens**.
- OpenID Connect (OIDC) is an identity layer built on top of the OAuth 2.0 framework. It allows third-party applications to verify the identity of the end-user and to obtain basic user profile information. OIDC uses JWTs, which you can obtain using flows conforming to the OAuth 2.0 specifications. Simply saying, it adds an **additional token** called **ID Token**.

### Anology
Let's understand that like this: a Guest check at a hotel reception with ID card, then reception give him a digit key which can unlock a hotel room, or visit gym, swiming pool, etc. Hotel room lock just accept the key and don't care who use it. Guest is User, Reception is OpenID IDP(Authentication), digit card is Access token. 
Now, there is a apecial service, kid care, which need digit key, and User information to show up as well. OAuth can do nothing about this, but OIDC can do it!

### OIDC and JWT
Each time you need to log in to a website using OIDC, you are redirected to your OpenID site where you log in, and then taken back to the website. For example, if you chose to sign in to Auth0 using your Google account then you used OIDC. Once you successfully authenticate with Google and authorize Auth0 to access your information, Google sends information back to Auth0 about the user and the authentication performed. This information is returned in a JWT. You'll receive an access token and if requested, an ID token.

The OIDC specification defines a set of standard claims for JWT. The set of standard claims include name, email, gender, birth date, and so on. However, if you want to capture information about a user and there currently isn't a standard claim that best reflects this piece of information, you can create custom claims and add them to your tokens.

### How is OIDC different from OpenID2.0?

OIDC has many architectural similarities to OpenID 2.0, and in fact the protocols solve a very similar set of problems. However, OpenID 2.0 used **XML** and a custom message signature scheme that in practice sometimes proved difficult for developers to get right, with the effect that OpenID 2.0 implementations would sometimes mysteriously refuse to interoperate. OAuth 2.0, the substrate for OpenID Connect, outsources the necessary encryption to the Web’s built-in TLS (also called HTTPS or SSL) infrastructure, which is universally implemented on both client and server platforms. OpenID Connect uses standard **JWT** data structures when signatures are required. This makes OpenID Connect dramatically easier for developers to implement, and in practice has resulted in much better interoperability.

### More concrete way to understand the concepts
Authentication = Identifying user
Authorization = Accessing APIs

## JWT

JSON Web Token (JWT) defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally **signed**.

- JWTs can be **signed** using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.
- Although JWT can be encrypted to also provide secrecy between parties, It will focus on **signed** tokens.
- It need to be used with HTTPS connection.

### TWO common usage scenarios

- **Authorization**: This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token. Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.
- **Information Exchange**: JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed — for example, using public/private key pairs — you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

### JWT structure

Three parts separated by dots (.), which are:
- Header
- Payload
- Signature

Like this:
{% include figure.html path="assets/img/encoded-jwt3.png" class="img-fluid rounded z-depth-1" width="80%" %}

Header and Payload are just plain text that get encoded, but not encrypted. So everyone can read them. JWT just focus on signed.

If you want to play with JWT and put these concepts into practice, you can use jwt.io Debugger to decode, verify, and generate JWTs.

### How JWT implement authorization

JWTs are self-contained, all the necessary information is there, reducing the need of going back and forward to the database. JWTs can container Authorization informaition.

### Client-side/Stateless Sessions
The so-called stateless sessions are in fact nothing more than client-side data. 
Most of the time sessions need only be signed. In other words, there is no security or privacy concern when data stored in them is read by third parties.

## OAuth 2

The apps which using OAuth2 usually are web server, browser-based SPA and mobile apps. OAuth 2 provides **Authorization Code** to grant authorization. OAuth still provide other grant type like Password, Client credentials, PKCE.

Here will display an Web server App example which can explain the main process of OAuth2. The process in SPA, mobile App have a little bit difference.

Let's say, you are developing a MobilePrinter Website App which using OAuth process. This App can help users print photo in Other server, like Google photos.

### Roles
- The Third-Party Application, "Client", MobilePrinter Website App
- The API: "Resource Server", Visiting Google photos
- The Authorization Server: Google
- The User: "Resource Owner"

### Creating an App
MobilePrinter must first register a new app with the service. 
- You must register a redirect URI to be used for redirecting users to for web server.
- You will get Client ID and Secret.

### Authorization process

MobilePrinter Website use Client ID and Secret communicate with the authorization server, (Google).
- Create a "Log In" link sending the user to:
`https://authorization-server.com/auth?response_type=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&scope=photos&state=1234zyx`
- The user sees the authorization prompt (Allow or Deny) from authorization-server.
- If the user clicks "Allow," the authorization-service redirects the user back to your site with an authorization code: 
`https://example-app.com/cb?code=AUTH_CODE_HERE&state=1234zyx`
- To Get an Access Token, Your server should exchanges the authorization code for an access token by making a POST request to the authorization server's token endpoint:
```
POST https://api.authorization-server.com/token
  grant_type=authorization_code&
  code=AUTH_CODE_HERE&
  redirect_uri=REDIRECT_URI&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET
```
- The server replies with an access token and expiration time
```json
{
  "access_token":"RsT5OjbzRn430zqMLgV3Ia",
  "expires_in":3600
}
```

### PKCE

Single-page apps (or browser-based apps) run entirely in the browser after loading the source code from a web page. Since the entire source code is available to the browser, they cannot maintain the confidentiality of a client secret, so the secret is not used in this case. 

The flow is based on the authorization code flow above, but with the addition of a dynamically generated secret used on each request. This is known as the PKCE extension.

> Note: Previously, it was recommended that browser-based apps use the "Implicit" flow, which returns an access token immediately in the redirect and does not have a token exchange step. In the time since the spec was originally written, the industry best practice has changed to recommend that the authorization code flow be used without the client secret. This provides more opportunities to create a secure flow, such as using the PKCE extension. 


## FAQ

- Common use case for JWT?
- What is Cross-Site Request Forgery (CSRF)?
- How is OIDC different from OpenID2.0?

### What is Cross-Site Request Forgery (CSRF)?
Unlike cross-site scripting (XSS), which exploits the trust a user has for a particular site, CSRF exploits the trust that a site has in a user's browser.
In a CSRF attack, an innocent end user is tricked by an attacker into submitting a web request that they did not intend. This may cause actions to be performed on the website that can include inadvertent client or server data leakage, change of session state, or manipulation of an end user's account.

## References

[jwt.io](https://jwt.io/introduction)
- [OAuth2 simplified](https://aaronparecki.com/oauth-2-simplified/)