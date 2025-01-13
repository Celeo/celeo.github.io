---
title: Writing a website in Rust
date: 2025-01-12 16:27:07
tags:
- programming
- rust
---

And how it compares to the previous sites I've written in other languages.

<!-- more -->

## Background

I like aviation. I'm a member of the Virtual Denver ARTCC of [VATUSA](https://www.vatusa.net/), a division of [VATSIM](https://vatsim.net/), where I am the Jr Staff member responsible for maintaining the group's tech infrastructure. Namely, that means making, maintaining, and updating a website. The previous site was written by someone who moved onto another subdivision, and while it worked fine 99% of the time, I was nearly entirely unable to push fixes or make changes. This was in part due to the website running across several different Kubernetes pods, all hosted in a shared cluster that I had very limited access to. When seeking to make a new site, I had a few objectives:

- Provide a website solution for the vZDV VATUSA ARTCC.
- Be both fast and lightweight.
- Be easy to use.
- Follow good software development practices.
- Be easy to develop, deploy, and run.

## Enter: Rust

I really like the [Rust programming language](https://www.rust-lang.org/). _Really_ like it. It does so much so well:

- Strongly and statically typed
- Compiles to a binary
- Excellent tooling, both built-in and community-built
- Fantastic error messages
- Great first- and third-party libraries
- Runs fast and safe

Previously, I've written simple websites in Go, Java, JavaScript, and PHP, and a pretty large site in Python. Given my appreciation of Rust, I choose it as the starting point for this new site.

## Picking libraries

I've followed [Are we web yet?](https://www.arewewebyet.org/) for a while and read plenty of discussions about the best Rust web frameworks. I selected [Axum](https://github.com/tokio-rs/axum) for its performance and ergonomics.

For data persistence, I selected (again, with a _strong_ goal of simplicity) SQLite with [sqlx](https://github.com/launchbadge/sqlx) as the library to access it alongside raw SQL statements that I'd write and maintain rather than use an ORM or query builder.

The third big piece was how I'd render HTML. Not wanting to bring in a full SPA/MPA JS app, I decided to throw back to my Python days and use an HTML rendering engine to feed full HTML documents to browsers with [minijinja](https://github.com/mitsuhiko/minijinja) to have familiar syntax to Jinja2 in Python.

Toss in some caching, emailing, error handling, JSON and TOML processing, and a small collection of other things, and I had myself a website.

## Organizing the code

I knew that I would need both a website and a Discord bot to replace the existing tech. I could have the Discord bot communicate with the website via REST API calls, but given that they'd both be written in the same language and I didn't want to have to _create_ a separate REST API in the website, I decided to have the discord bot directly read/write to/from the database. I put a chunk of shared code to read the combined app configuration and access the database into a library, turning the project into a Cargo workspace (i.e. monorepo): I'd have a shared library, a website, a Discord bot, and a separate "task runner" to do scheduled tasks like pulling data from upstream data sources. This works quite well, and I don't have to fuss with any sort of weird versioning between the library and binaries; it just works.

## The good

As expected, the parts of Rust that I like (most of it), I still very much liked working on this website. Dealing with data was great with Rust's `iter` functions, working with the database was smooth enough (after applying SQLite tweaks to make it better, but that's outside of Rust), checking for nulls (`None`) was easy, making the app fast and safe was free, and more. Working with Cargo made everything easy, and I was able to `scp` the 3 binaries to my server and run them directly - no fussing around with Docker or installing a runtime on the server.

## The bad

My main complaint doesn't _directly_ have to do with Rust, rather with most compiled languages, but was the chief source of frustration and friction when building the app. Whenever I made a change, I'd have to stop the server and rerun, waiting for the compilation and linking. Thankfully this was only a few seconds, but after each change? It adds up, quickly. Thankfully I got the auto-reload feature in the templating library working (eventually; it's not simple) so I could make changes to templates and just refresh the page. But still, needing to pass any additional or different information to the template required stopping and rerunning, again incurring the cost of waiting for compilation.

Having to wait for compilation these days, now that the site is done and deployed, seems a small price to pay for being able to make changes and push them to my server without fuss or worry. Given the nature of what I was using the site for - pushing full HTML documents rather than supplying a REST API that a JS SPA would consume - I'm certainly not faulting tokio/axum/minijinja; I guess it's just the nature of what I was doing. When thinking back to building sites with Python, template changes were automatically uptaken, debugging was easier and more powerful, and there was no compilation step to wait for. Alas, Python came with it's own (IMO) shortcomings, of course - deployment was harder (basically requiring Docker), changes were less safe, the code was harder to come back without types, etc.

## Conclusion

I made a site with Rust, and I don't regret it.

I suppose I'm still ... watching and waiting for what I'd consider a perfect programming language. I'd want something quite close to Rust, but with garbage collection (I generally have no use for _not_ having GC) to make the code simpler, and near-instant compilation time like Go. Some readers might reason: why not just use Go, then? I like parts of Go (the tooling especially) and dislike others (syntax, unfortunately). Many people like Go, and that's great! I wish writing Rust was easier, or writing Python was safer. Zig is the new hotness, but I'm happy with Rust as a systems programming language. I suppose I'm looking for my next favorite "general" application programming language. Is it already out there - Lua, Ruby, Nim? - or still to come? I don't know. Should be exciting!
