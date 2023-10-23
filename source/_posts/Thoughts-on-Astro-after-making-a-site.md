---
title: Thoughts on Astro after making a site
date: 2023-10-23 12:07:04
tags:
- programming
---

I've written an interactive website with [Astro](https://astro.build/); here are my thoughts.

<!-- more -->

Link to repo: [github.com/celeo/zdv-training-scheduler](https://github.com/Celeo/zdv-training-scheduler).

Namely, I have a MPA with React components for interactivity. Other than the Privacy Policy page, all page content includes at least some client-side rendering. The navbar buttons are a React component. There's a React component on the homepage. The scheduling, admin, and preferences pages are entirely React.

I'd like to make an early note: it's entirely possible that I am not using Astro correctly. Perhaps I should be using Solid or Preact, or Alpine and HTMX, or something else entirely.

### The good

There are several things about Astro that I really like:

- No/little configuration required
- "Frontend" and "backend" code in a single project
- File-based routing

Starting an Astro project is easily done with their CLI prompt program, which is both easy to use and clearly designed to be _very_ friendly. Adding node SSR, React, tailwind, etc. are each a single command, with no or only a tiny bit of config changes. Everything just works, which in the frontend space is remarkable.

Not needing to split my project into two explicit sub-directories for backend and frontend code is nice. Since the entire site is in TypeScript, I'm able to use types in both places. As long as I don't import a file that uses Node functionality from client-side code, it all works very well.

This is the first time I've used file-based routing. For the pages that the user will visit, I've found it very intuitive and simple to use.

### The bad

- Unable to use some frontend component libraries
- File-based rounding
- Weird handling of environment variables between dev server and production builds
- Learning curve for client vs server Astro code

While I am able to use React components styled up with tailwind (thanks to Flowbite for the designs), I'm _not_ able to use component libraries like [Mui](https://mui.com/), as their approach to component styling is not supported in Astro.

File-based routing appears in "good" and "bad" - what gives? Well, for pages that the user will land on, I think file-based routing works very well, as noted above. For _server_ endpoints, however, I'm not a fan. I can add a `src/pages/api/foo.ts` file and call `GET /api/foo` from my API. I can do any _kind_ of request to that file (GET, POST, etc.) and pass in query parameters without issue. If, however, I then want to follow good API design and be able to call `DELETE /api/foo/4`, I need to rename `src/pages/api/foo.ts` to `src/pages/api/foo/index.ts` and add `src/pages/api/foo/[id].ts` to receive the new responses. Functionally it works fine, but I find it annoying.

Environment variables have sapped around 5 hours of my time on this project, resulting in hapless internet searching and a post in Astro's Discord help channel that went unanswered. In short, environment variables are automatically loaded from adjacent '.env' files in the development environment and are available everywhere in the site, while _building_ the site either bakes the secrets into the generated HTML/JS or ignores them entirely. I decided to forego env vars entirely, instead favoring a file config that's loaded through Node functions at runtime. If you'll be using Astro to build a site that has a secret (database password, third-party site secret token, etc.) and won't have full control over the built files (i.e. won't be publishing a Docker container with the site) I recommend avoiding env vars as much as possible.

While I can look back at the code and easily see which parts are for the client and which are for the server, this line was quite fuzzy when learning. Due in part to having frontend and backend code in the same project and same language (I usually avoid server-side TS/JS in favor of better languages) and having Astro handle "client" and "server" pages, getting used to what code is executed where is challenging. Especially in ".astro" files, the "header" where Astro runs the code as part of the SSR (for applicable setups) process, you're writing server code in what basically looks like a client page (because that's what it _will_ be).

### Closing thoughts

I think Astro is a wonderful choice for a static site and sites with limited interactivity. I cannot say for sure, but using Astro without a "typical"/modern frontend library like React, Solid, etc. might work quite well; it might actually be a good opportunity for client-side-only Vue (no compile process). I could probably build a site without server endpoints, instead using Alpine or Intercooler and ".astro" files to generate and return HTML rather than JSON.

If, knowing what I now know, would I have built this site with Astro? No, probably not. But that doesn't mean I regret this learning experience, because I certainly do not. If I look to build another site, one with less interactivity and more static information, in the future, I'll happily reach for Astro. For more people, I think whether or not Astro is the best choice ("best", because I think it's at least a "good" choice for most use-cases) comes down to what your personal opinion on web "site" vs web "app" is. MPAs/webSITEs are easily done on Astro. WebAPPs might be better handled by Next, Nuxt, SolidStart, etc.

I haven't mentioned the "BETH" stack using Bun, because I haven't used it yet.
