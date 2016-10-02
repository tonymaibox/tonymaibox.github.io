---
layout: post
section-type: post
title: JWT this down...
category: tech
tags: [ 'tech' ]
---
JSON Web Tokens (JWT) are great, but not as great as [_insert other web application security platform for transfering information_].

Okay, so I didn't exactly tell you something you didn't know already: "There is no security in this life, only opportunity," as said by U.S. Army General Douglas MacArthur.

<img align="center" src="https://camo.githubusercontent.com/8a470031ff0b8df360e9501623e06c25a692b851/687474703a2f2f692e696d6775722e636f6d2f634e456c566f662e6a7067" alt="...">

Security is a perception, one doesn't really question security until they...well, start to question it or begin to doubt it. Still, it doesn't mean we shouldn't place security measures in our web applications to verify authorized usage. Enter JSON Web Tokens (JWT, or "jots").

### JWT (JSON Web Token)

JWTs are a great way of authenticating users to their requested servers, specifically API (Application Program Interface) databases. When accessing an external API (separate from one's own domain or server), we enter Cross Origin Resource Sharing which has its own protocols. If the API server in question does not wish to allow for cookies, then JWT is a great solution.

JWTs are stateless. Meaning, they don't hold state-ful data. They do make information exchanges but not of the state-ful variety. Most JWTs will carry information about the user who "claims" to have access to its requested server location/URL.

A web request with JWT looks like this:

<img align="center" src="https://cdn.auth0.com/content/jwt/jwt-diagram.png" alt="...">
<br />

### JWT Structure

A typical JWT looks like this:

aaaaaa.bbbbbbbbbb.ccccccccccc

or

<pre><code class="html">
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MX0.o5x69ylYxe4r1AeSXQ6NqgUXktfvdTOvULm2aKatp0s
</code></pre>

JWTs consists of three parts as you can sort of make out in the above string (separated by "."):
  1. Header
      - 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9'
  2. Payload
      - 'eyJpZCI6MX0'  
  3. Signature
      - 'o5x69ylYxe4r1AeSXQ6NqgUXktfvdTOvULm2aKatp0s'

#### JWT Header

The header is typically:

<pre><code class="java">
{
    "alg": "HS256",
    "typ": "JWT"
}
</code></pre>

Here, the information provided is the hashing algorithm to encrypt it's "secret key" and the type of token being passed, "JWT". The different kinds of algorithms used is HMAC SHA256 or RSA.

#### JWT Payload

The Payload contains specific claims in the form of key-value pairs that make up the data exchange between user and server. This is where information is made or claimed about the user. For instance, a payload can look like this:

<pre><code class="java">
{
    "id": "1",
    "name": "Me"
    "admin": true
}
</code></pre>

When I pass this claim to the server via a login form, and if valid, I can access the API as if I was actually on the server.

For both the header and payload, Base64Url encoding is applied to make the first two parts of the JWT string (which looks like some random set of characters, but it's not - more of this later).

#### JWT Signature

The JWT signature is created by taking the encoded header, encoded payload, a secret (key), the algorithm used (specified in the header), and sign it all. It would be created like this:

<pre><code class="java">
    HMACSHA256(
      base64UrlEncode(header) + "." +
      base64UrlEncode(payload),
      secret)
</code></pre>
<br />

#### JWT De-construction

This all seems simple enough to understand. If you want to see or create a Ruby on Rails application with JWT authentication, here's a great blog post by one of my instructors to get you started: [Sophie DeBenedetto's "JWT Auth in Rails, From Scratch"](http://www.thegreatcodeadventure.com/jwt-auth-in-rails-from-scratch/). And you're looking for a React-Redux front-end for your application, she also wrote another amazing tutorial: ["JWT Authentication with React + Redux"](http://www.thegreatcodeadventure.com/jwt-authentication-with-react-redux/). If you want to verify that you created your token correctly or not, here's a simple tool to use: [JST.io](https://jwt.io/). Paste your token, and you'll see the breakdown of your header, payload, and even be able to apply your "secret key" (keep it secret to yourself and team!) to verify that it's an actual JWT object.

### Now what?

Simple enough? Actually, it is, which does make me question its security. These JWTs could be included in HTTP headers or URLs for users to access. While we should be making such links single or one-time use only, JWTs aren't totally immune to hijacking or attackers. A couple of things to consider:

1. **Decision between localStorage or sessionStorage**
  * The JWT token lives on the browser's local storage. As a web app developer, we have to decide on user experience versus user security. It's a delicate balance. user experience could be bolstered if a user just has to sign onto an app, like a mobile one, just once. But for security's sake, it might be better if they needed to sign on everytime they're trying to access the web server. A browser's localStorage is persistent, but it's sessionStorage is not. "sessionStorage" clears upon closing of the browser. This has become the suggested method of storing the JWT. But it's a consideration to ponder.
2. **Depending on how you view the above bullet point, set expirations**
  * You can add an additional reserved "claim" to the payload. "exp" uses Unix time to set token expiration.
3. **Key IDs or "kid" rather than Algorithm or "alg"**
  * Key IDs are a way of adding an extra layer of authentication for this authentication method. In substitution of the "alg" indicator in the header, using "kid" which stores the algorithm information privately on the sides of client and server makes it less convenient for backwards-deconstruction of a JWT token attack. 

That's all from me for now. These are a few things I'm working on and considering for my next project.

#### Sources
  * [Sophie DeBenedetto's blog post - JWT Auth in Rails, From Scratch](http://www.thegreatcodeadventure.com/jwt-auth-in-rails-from-scratch/)
  * [Sophie DeBenedetto's blog post - JWT Authentication with React + Redux](http://www.thegreatcodeadventure.com/jwt-authentication-with-react-redux/).
  * [JWT.io](https://jwt.io)
  * [AuthO - JSON Web Tokens](https://auth0.com/learn/json-web-tokens/)
  * [AuthO - Critical Vulnerabilities in JWT Libraries](https://auth0.com/blog/critical-vulnerabilities-in-json-web-token-libraries/)
  * [Toptal.com - JWT Tutorial](https://www.toptal.com/web/cookie-free-authentication-with-json-web-tokens-an-example-in-laravel-and-angularjs)
  * [Stormpath.com - Use JWT The Right Way](https://stormpath.com/blog/jwt-the-right-way)

_Please comment to share your thoughts, help me clarify mine, or any insights you have on the matter. Thanks! And have a great day._

