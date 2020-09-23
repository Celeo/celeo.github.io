---
title: Getting started with Rust
date: 2020-09-22 16:33:35
tags:
- rust
- programming
---

Everything you need to get started with programming in [Rust](https://www.rust-lang.org/).

<!-- more -->

## Getting it

First, you can try Rust without installing it on your system. There are 2 ways to achieve this:

1. The [Rust playground](https://play.rust-lang.org/) allows you to write and run Rust in your browser
1. If you use [Docker](https://www.docker.com/), there are official [Rust images](https://hub.docker.com/_/rust)

If you're ready to get started with an actual installation, the Rust team recommends [rustup](https://rustup.rs/), which is an easy-to-use toolchain installer. What's a toolchain? Well, [this is what Wikipedia says](https://en.wikipedia.org/wiki/Toolchain), but for a new Rust user, as long as you're using a popular operating system, all you need to get is "stable". This is the current stable version of the Rust toolchain for your system. Installing rustup installs this too, so don't worry about it.

Once you have rustup, you'll need to modify your path, so do that however your shell handles it. Don't know? No problem, just search "[your shell] add to path" on your favorite search engine or check the [Rust documentation](https://www.rust-lang.org/tools/install).

And that should be it for the installation. If you haven't worked with compiling C at all on your system, you may get an error message about "linker `cc` not found". If that happens, you'll probably be good if you install "gcc" for your system (look it up).

## Getting started

When you're just starting off, you can absolutely use the Rust playground or any text editor that you like (anything from nano/vim to an IDE). If you're using an IDE, it may have Rust support. However you get code into a text file, you can compile with `rustc [file name]` to get an executable of the same name.

When you're ready for the next step and have Rust installed on your system, you'll want to start learning [cargo](https://doc.rust-lang.org/cargo/index.html), which is the Rust package manager + build tool. Running `cargo new [project name]` will set up a new directory with a few files, bootstrapping your Rust projects. Don't worry, this isn't like some of the build tools in the front-end ecosystem that make a directory with a ton of files, all full of information. Cargo just creates the minimum; the rest is up to you. When you start a project with cargo, you can use `cargo build` instead of `rustc [file name]`, which should speed you up.

## Learning resources

In case you haven't heard this already, here it is:

> Rust has a higher learning curve than most programming languages

Don't let that turn you away, though! To combat that learning curve, there are a ton of high-quality resources that the language team and community maintain. Check these out:

- The official "Rust Book": <https://doc.rust-lang.org/book/>
- Learning Rust: <https://learning-rust.github.io/>

When you get to using other crates (libraries), you can always punch the name of the library into [docs.rs](https://doc.rust-lang.org) and you'll see the auto-generated documentation!

## Communities

Want to chat about and get help with Rust with others? The Rust team has [a page for that](https://www.rust-lang.org/community)! Additionally, there's a [Rust](https://old.reddit.com/r/rust) subreddit, which gets a lot of good information. Finally, a good amount of Rust news makes it to [Hacker News](https://news.ycombinator.com/).

## Enjoy

Have fun! It can be a rough journey at times, but take advantage of the many passionate people who have come before, and know that you can do it too!
