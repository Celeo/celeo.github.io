---
title: Initial thoughts on Svelte
date: 2021-01-18 17:04:23
tags:
- programming
- webdev
---

Initial thoughts on Svelte from someone who's played with many frameworks, likes compile-time processing, but has been (mostly) happy with React for several years.

<!-- more -->

## Intro

I've heard murmurings of Svelte, about how it was the hot new thing, and how it would disrupt the established React-focused web development patterns that blah blah blah. It's new. In the webdev world, something _being_ new is itself nothing new.

Briefly, I want to mention the path I took to becoming comfortable with the current/modern web development process. I started with jQuery, like most people. I never really got into jQuery UI. I wanted to look into the new hotness of creating JavaScript-focused websites instead of (what's now called) SSR (which was the default back then, instead of the SEO-enabling hotness that people are adding _back_ to modern webapps). I found trying to learn node, Angular, and TypeScript all at once just too much for what I wanted to do. I tried React, but it didn't help with the node-driven development process, and jumping from "I write complete HTML pages" to "if you're not using 100 components, you're wrong" was too much. I then found Vue, and was actually able to get into its mindset. Vue let me start simple, by just adding extra functionality to existing sites, and then eventually move into SPAs, `.vue` files, node, webpack, etc. From there, I went back to React some time later as I wanted to become at least proficient due to its increasingly-common appearance in the software industry. Taking what I learned from Vue with regards to components, node, webpack, etc. let me get into React with much less frustration and confusion than I had previously had. Given this lower barrier to entry, I was able to move forward, and I have been using React for professional and hobby web development for years.

For the most part, I'm happy with the developer experience I get with React, webpack, eslint, prettier, jest, and TypeScript. In a vast ecosystem of seemingly thousands of tools, many of which do the same or similar things with different approaches, finding a "stack" (maybe, "group"?) of tools that work _for you_ and enable _you_ to build apps is a very important task. For me, it's that bunch that let me write webapps without wanting to delete node and throw money at something like [Gemini](https://gemini.circumlunar.space/) (though I probably perhaps still should). Why then, look outside that space of things I like for something new? It's half because that's something I've done for years to large success, and half because that's sort of expected with the webdev world.

## Enter, Svelte

First off, take a look at the [official tutorial](https://svelte.dev/tutorial) - it's really quite great. Next, take note that Svelte is the highest in [satisfaction and interest](https://2020.stateofjs.com/en-US/technologies/front-end-frameworks/) in the 2020 State of JS survey (which itself is great, and this year's results site is _beautiful_).

I wanted to combine my learning Svelte with learning [snowpack](https://www.snowpack.dev/), another [high-ranking tool from the survey](https://2020.stateofjs.com/en-US/technologies/build-tools/), being top in interest and second in satisfaction (snowpack uses esbuild, which got #1 in satisfaction). I'm still not sure if that was an intelligent decision, but it's the one I made. It seems to work okay, but the Svelte tutorials seem significantly focused on rollup (which is something I've never used before; I used webpack). Snowpack has a scaffolding tool calling [create-snowpack-app](https://github.com/snowpackjs/snowpack/tree/main/create-snowpack-app/cli) (CSA) that can be used to set up projects in a number of frameworks, including Svelte. I also took time to go through the first few chapters of Svelte's interactive tutorial.

The first thing I noted when starting learning Svelte is that it _writes_ like Vue `.vue` files, where the template, logic, and styling are all in a single file. In React, the "template" (JSX) and logic are usually in the same file, but the styles are often in separate files that webpack takes care of bundling up. Additionally, because React uses JSX instead of this more "template" approach (also seen in Vue), there's a loss of clarity between the "how" and the "what". (I'd say that this is just, like, _my opinion_, but this entire blog is my opinion, so ... /shrug)

I'm going to write there that Svelte performs fine in most cases that I've done so far, though those cases aren't particularly complicated. Svelte's reactivity with the `$:` operator is simple enough to do, but there's really any _perceived_ benefit _to the developer_ over Vue's computed properties or a `useEffect` hook with dependencies in React. Obviously they all perform differently at runtime, but from a developer's view, eh. What Svelte does **not** do well with, is the "extra" tools that I've becoming happy and comfortable with.

## Problems with TypeScript and ESLint

Writing Svelte components (are they still called components?) in TypeScript blocks the developer from using ESLint on those files. Yeah. Not wanting to give up either, I've opted _for now_ to give up TypeScript and still use ESLint (snowpack lets me write ES201# which gets transpiled to the old nasty).

## Problems with Prettier

I added prettier and Svelte's prettier plugin to my project, configured VSCode to autoformat on save, and saved, which promptly blew up parts of my template markup, which caused my component to fail to render. No problem, I can fix that easy enough, right? No. I spent more time fighting prettier and Svelte disagreeing than I spent **writing the webapp**. I _can_ make do without Prettier, and just use ESLint to enforce (and auto-fix) styling things that prettier covers (even though prettier does it better) as the stuff I'm working on is side-project (at best).

## Problems with snowpack (maybe)

For this, I don't know if it's the combination of Svelte and snowpack that's being annoying, or what. This frustration has to deal with errors. Sometimes, when my component has an error, I'll get an error screen in my browser pointing it out (like React). Sometimes, the webapp will _start_ to render/mount/whatever and the site's background color will be set, but there won't be anything actually there. I have to go to the terminal running snowpack to see the error. This isn't dissimilar to webpack, so it's not a problem.

The main frustration comes when **I can't figure out _when_ each of these scenarios has occurred** without looking at the browser, refreshing a few times, then looking at the devtools console, then looking at my terminal window. Big time loss.

## Next steps

There are tools I'm willing to swap out or abandon for learning Svelte, like snowpack for rollup, but there are things that I _won't_ leave behind in modern webdev, like ESLint and TypeScript. If Svelte isn't mature enough to support these extremely commonplace tools, then perhaps it's not mature enough for me to learn it.

I'm not trying to sound boisterous or authoritative, or make sound like anyone _remotely_ important in the webdev world, but just an honest statement - if it's not ready, then that's **fine** - I'll come back later. But with Svelte's 3.0 release just days ago, I'm really confused _why_ these (again, super common) tools don't work. Does the Svelte community not like them? Do they not need them (like in Elm)? _I_ like them, though, and _I_ need (kinda) them, so maybe Svelte just isn't for me (or at least, not yet?).

Regardless, Svelte has some really neat concepts, and I wish the absolute best for the project.
