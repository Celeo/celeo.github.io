---
title: Quick thoughts on language tooling
date: 2022-07-23 20:40:12
tags:
- programming
---

I like Zig's syntax more than Nim's, but Nim is much further along.

Yeah, Zig isn't 1.0 yet, but it seems like most new (relative) languages having the tooling squared away.

Zig does cross-compiling better than Rust, probably almost as good as Go, so that's cool.

Before Go sorted out package versions, it still had go get, which worked well enough for most people.

Looks like Zig tells people to go the "./vendor/" route, cloning in your dependencies. Not awful, but without at least some tooling, kind of a bummer.

I like having a standard, official package management solution. Python has many usable package managers, all of which have their own merits. It took me years to find `poetry`, which is my (current) favorite.

Go had a lot as well, before they sorted everything out.

Rust? `cargo`. Done.

Heck, even JavaScript has `npm` leading the pack, with some folks on `yarn` or `pnpm`. And, `npm` rips off the features that the other managers introduce after a while, so there's no major reason to switch off of `npm` for most people.

Nim has `nimble`.

Elixir has `mix`.

Technically you could install all Python packages to your global scope, but that is a pretty sure-fire way to dork up your entire system.

Zig likes to promote that their language compiler is also a compiler for any C program, and that's dandy, but still - fragmentation. Every Zig project comes with a build file that is 22 SLOC!

Rust allows the customization of its build process via a build file, but it's rarely required!

Go has programs that can bake extra stuff into the binary. I haven't used one in a looong time, but I remember them/it being fairly simple.

Python ... doesn't compile, which will probably forever bug me.

TypeScript doesn't either, not technically. Can webpack it down to a all-in-one runnable node file and distribute that, though still requires the user to have the runtime installed. Most devs will, sure, but users? Very unlikely. Deno moves to solve that with their executable builds, but they're still sort of buggy.

Java .jars don't count if Python doesn't.

Plus like holy crap, ever unzip an enterprise jar? It's like GBs of stuff. I stopped the unzip one time after like 50 GB.
