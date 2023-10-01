---
title: Implementing an OAuth 2 flow in Astro
date: 2023-09-30 22:45:52
tags:
- programming
---

I've been working with [Astro](https://astro.build/) lately, and it took me a while to figure out how to implement an OAuth 2 client. Hopefully I can make it faster for you.

<!-- more -->

First off, I'm using Astro with the Node adapter for SSR. You can use any SSR adapter you want, though you'll definitely need SSR.

The standard OAuth flow is as follows:

- Register an application with the site
- The user clicks on a login button/link
- Generate a URL to send the user to, using your OAuth application information
- The user logs into that site and authorizes your site
- The user is redirected to your site
- You get a `code` query parameter in the URL
- You exchange the code for an access token
- You use the access token to get information about the user and perform actions on their behalf

All of these can be performed with simple HTTP requests, or you can use [a library](https://www.npmjs.com/package/oauth) to help you.

Now, in Astro.

When the clicks on your login link, you could send them to a server endpoint that returns a redirect, or you could point them to a regular `.astro` page
that generates the URL, stores it in the DOM, and has a JavaScript function on document ready that loads the URL from the DOM and performs the redirect.

When the user returns to your site, your callback `.astro` page has a fair bit of processing to do. You can handle exchanging the code for the access
token and using it to get information about the user, all before loading the page. Do note that if the site you're interacting with is slow, you'll
probably instead want to defer some of that to async processing via a HTTP request from the client via JavaScript.

As part of generating the URL to send the user to the OAuth server site, you can include a `state` variable. I'm using
[nanostores](https://docs.astro.build/en/core-concepts/sharing-state/) both in the login page and the callback page, to ensure that the flow isn't being
messed with.