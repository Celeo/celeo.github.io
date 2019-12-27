---
title: Thoughts on Rust
date: 2019-12-24 14:54:52
tags:
- programming
- rust
---

Thoughts on the [Rust](https://www.rust-lang.org/) programming language after 6 months of learning and use.

<!-- more -->

## Outline

* My background
* Learning Rust
* The good parts
* The bad parts
* Moving forward

## My background

I've always preferred high-level languages. Java, Python, JavaScript, C#, etc. I spend 8 or so months learning C++ in college, but haven't ever used it outside that class. I find that for the side projects that I work on and the work that I do, I need a language that will help me get things done quickly and accurately, instead of allowing (or requiring) me to micro-manage things like memory usage. To this end, I haven't needed to really, _actually_ learn a lower level language like C/C++.

## Learning Rust

There were many things that drew me to Rust, including it repeatedly winning StackOverflow's "most loved language", the great things that people were saying about the language and ecosystem on reddit, HackerNews, and other forums, and the compile-to-binary-that-runs-fast aspect (solving for the few things that I miss having when using Python). Additionally, I had wanted to learn more about functional programming, but had been burned by trying to learn FP through a solely FP language called Idris. I'd read that Rust had FP-like aspects, but made them much more approachable.

I'm not unique in writing that learning Rust was very tough. As someone who hasn't written in a lower level language in years, I hadn't encountered many of the problems that Rust was solving for, like making sure memory was freed at the right time, making sure memory wasn't accessed by multiple procedures, etc. The languages I used always had runtimes or GCs that handled those processes. Suddenly, I had to think about things that I hadn't thought about, in ways that didn't really make sense, as I hadn't had the pain points of _not_ having the language help me with those issues. Not only did I have to learn about the issues, but I also had to learn about how Rust solved for them.

There are two really great things about learning Rust: the documentation and the compiler.

### Documentation

Rust's documentation is really outstanding. I don't think that the language would have reached the popularity that it has and continues to expand on if the documentation wasn't at the quantity and quality that new and current users enjoy today. There are many [official writings](https://www.rust-lanng.org/learn) on learning the language and ecosystem, and also a lot of community-maintained resources that you can find across the web. Learning Rust is not easy, but reading the documentation makes it feel like a much less daunting prospect - you're not left alone to struggle through it all.

### Compiler

Rust's compiler is really, really impressive, and not only because of the borrow checker. Whenever I write in another language after having experienced the Rust compiler's error messages, I experience a twinge of longing for those clear, explanatory, colored, and [indexed](https://docs.rust-lang.org/error-index.html). Any time the compiler complains about an error in your code, if you want more information, you can run the command `rustc --explain <error>` and get information not just on what tends to cause the error, but also how to most commonly resolve it. Although I had many more errors in my Rust code while learning than in most other languages, the compiler's ability to pinpoint where exactly the error was, explaining why the code was wrong, and also giving potential solutions meant that I had to search the web for answers less frequently than for other languages.

## The good parts

I just highlighted two huge positives, but there are certainly more.

Extending on the high-quality documentation for the language, I found a majority of the popular libraries that I reached for continued this spirit of being well-documented. Rust's compiler contains [rustdoc](https://doc.rust-lang.org/rustdoc/index.html), a utility for turning function and variable docstrings into actual webpages, complete with indexing, searching, and everything that I had become used to in the standard library documentation. Library authors were encouraged to use this functionality, as all of Rust's [public packages ("crates")](https://crates.io) get free documentation hosting through [docs.rs](https://docs.rs). All library authors have to do is write the docstrings, and the free-to-use Rust documentation site builds and hosts the documentation.

If you go looking into learning the language, you'll find many people writing about how open and welcoming the community was. I never really interacted with the community, preferring a "read-only" approach of just consuming information to fuel my learning, but from what I read, that ideal holds true.

Finally, [cargo](https://docs.rust-lang.org/cargo), the Rust package manager, is an absolute dream. It handles searching for, getting information on, and downloading dependencies, as well as building, running, and testing projects, and even [formatting](https://github.com/rust-lang/rustfmt) and [linting](https://github.com/rust-lang/rust-clippy)) to help programmers write better code. I wish every other language had a package manager like Cargo.

Also, [`unimplemented!()`](https://doc.rust-lang.org/std/macro.unimplemented.html) is awesome.

## The bad parts

Rust's compile time is a bummer. I totally understand that the compiler is getting faster month by month as very smart people work on it, and also that it has a lot to do, so I don't fault it for being slow. Unfortunately, it contributed to me putting Rust on the shelf instead of giving it a slot on my everyday tool belt.

Starting a new project in Rust is really quick (thanks, cargo!), but as soon as you start to add dependencies, like the outstanding data (de)serialization library [serde](https://serde.rs), compile times start to creep up. For the type of projects that I work on, it was pretty common to need JSON (de)serialization, HTTP calls, and logging. All of these libraries are outstanding, but compiling a no-op project for the first time with these libraries added meant waiting several minutes for everything to get ready. Note that subsequent compiles took significantly less time, as the compiler (I assume) cached the built libraries. This "caching" seemed to be local to the project I was working on, though, which meant that this initial compile effort was repeated for every new project I started, and when learning a language, that can be often. I would say that this initial compile time wouldn't be an issue for cloning down a large project and building it, but even adding what I'd consider pretty common utilities incurred a time cost more than I'd like.

And yes, I know that a few minutes isn't _really_ a lot of time.

The borrow checker is an incredible feature that is a huge part of the language. I enjoyed having it there when I wrote compliant code, and I appreciate everything that it does for the programmer. Learning to write code that satisfies the checker was a difficult task, but that's pretty much the biggest hurdle of learning the language. Unfortunately, even after spending months with the language, I'd still run into instances where some code I had written wouldn't be compliant, and I'd have to spend time satisfying the checker rather than actually moving forward with the project. I'd wager that this is, in fact, my fault, and not the compiler's, but it still was unfortunate to run into these walls when I just wanted to write a quick little something.

## Moving forward

Like I mentioned at the start, I never really had the _need_ for a language like Rust. Whether it's a good or bad thing, I still don't. Rust as a language and ecosystem continue to impress me, and I wish nothing but the best for the teams working on and using the language. I spent time writing a [number of small programs, libraries, and utilities](https://github.com/Celeo?utf8=%E2%9C%93&tab=repositories&q=&type=&language=rust) in Rust to help me learn as well as to have something to show for the effort. I certainly think that working with Rust was overall a positive experience, and I'd encourage anyone on the fence about devoting the time (for it definitely is a notable investment) to learning the language to do so.

Over time, though, I found myself reaching for Python where others wrote about reaching for Rust, and for Java or JavaScript where people wrote about using Rust. I found myself still not needing the speed and security that Rust has built-in. As a developer, doing what I do right now at work and at home, I don't need Rust. And I'm okay with that.
