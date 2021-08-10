---
title: Writing websites in Rust
date: 2021-08-09 17:57:59
tags:
- programming
---

You'll certainly end up with a fast, safe, and easy-to-deploy service, but is it worth the tradeoffs?

**WARNING**: this is a bit of a rant and not likely helpful.

<!-- more -->

Not for me.

I've been playing around with the new [axum](https://crates.io/crates/axum) library, which builds on top of [tokio](https://crates.io/crates/tokio) and [hyper](https://crates.io/crates/hyper) and accepts middleware from [tower](https://crates.io/crates/tower) among others. Sounds like a good time. Their [documentation](https://docs.rs/axum/0.1.3/axum/) seems well constructed, the tokio Discord server is active and helpful, and they have a good collection of [examples](https://github.com/tokio-rs/axum/tree/main/examples).

The service I wanted to create is very simple - a pastebin-style website. I'd use templates for the HTML, and an auth-less no-edit paste system with a "delete after" date. There are only 4 endpoints:

- index page
- view paste page
- new paste page
- new paste POST page

This is not hard, and not complicated.

I'm using [sqlx](https://crates.io/crates/sqlx) to handle the storage in SQLite. Again, _simple_. [askama](https://crates.io/crates/askama) handles the templates in a not-so-simple way, but it's not too complicated to connect with axum with the examples provided (though I had to basically write a custom implementation of the bridge function, as the code in the example didn't work for me). [uuid](https://crates.io/crates/uuid) is a simple way to create UUID v4s, though I had to play around with the feature flags to get what I wanted.

The list of dependencies I'm using can be seen [here](https://github.com/Celeo/pastey/blob/5a6837b9f585167c4c649b6f659970d1130232ca/Cargo.toml), though keep in mind I haven't slimmed them down at the time of writing. If I cannot slim them down, then there's a lot.

Now, the time penalty you pay for adding dependencies is only noticeable on the first compiles (via `cargo b`, `cargo clippy`, _and_ by VSCode). It's not bad. What _does_ annoy me is the penalty I am paying for each compile: 5 to 10 seconds. Each. Change.

Now sure, there's a lot going on behind the scenes for this, and I get a fast, safe, single binary deployable. Right now, I have some programs that are writing in Elixir and deployed via podman images rsync'd directly to a VPS - neither release-style building time nor size are an issue right now. It's the _developer UX_ that is rough. If I make a change to a template, that's a recompile. If I make a change to a route, that's a recompile. If I add a log statement, that's a recompile. Does that make sense? Yes. Is it nice? No. The size of that directory right now is 1.3GB. Yes, GB. The directory size of a Discord bot I run is 118*MB*.

Is Rust a good language? Yes, absolutely! I really like many of the things that the Rust language, ecosystem, team, and community do, and I absolutely plan to continue to use Rust in many different ways. But for web servers? Nah, I'm happy with Python.
