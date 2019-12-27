---
title: Nim
date: 2019-12-21 15:01:31
tags:
- programming
- nim
---

Thoughts on the [Nim programming language](https://nim-lang.org).

<!-- more -->

## Outline

1. First attempt to learn
1. Coming back
1. Working through Advent of Code
1. What I think now
1. Learning resources

## First attempt

I first started playing around with Nim about a year ago, having been drawn to it's Python-like syntax, speed, and compile-to-binary capabilities. I love the Python language, but I have grown far more fond of static typing in recent years, and Python's deployment strategies have always irked me. Nim seemed like a perfect language to solve for the problems that I had with Python, while not abandoning the parts I like (I've never had a problem with whitespace-dependent languages, for instance).

My initial impression was good - the syntax is simple enough, it ran fast, and the compiler pointed out if I did something wrong. I started to get tripped up when it came to objects though, as that's when I first came across Nim's ["unified call syntax"](https://nim-lang.org/docs/manual.html#procedures-method-call-syntax). If you're unaware, the following are all equivalent (taken from the link):

```nim
echo len "abc"
echo "abc".len
echo("abc".len())
e_ch_O(len("abc"))
```

Yeah, it's weird.

Unfortunately, the lack of uniformity soured the language pretty drastically for me. Combined with the `import` functionality, which is pretty different than Python, I decided that Nim wasn't for me. I ended up turning to learn [Rust](https://www.rust-lang.org/), still searching for a fast-to-run, compile-to-binary language (I'm not a big fan of Go, though it solves both of those requirements).

## Coming back

On Sept 23, 2019, Nim had its 1.0 release. First off, congratulations to the team for hitting that milestone!

I found out about the 1.0 release from reddit and figured that it was time to give the language another (better) chance. Installation was simple with the [choosenim](https://nim-lang.org/install_unix.html) (sort of like [nvm](https://github.com/nvm-sh/nvm) or [rustup](https://rustup.rs/)) tool (though I used [brew](https://brew.sh/) on mac). I spend some time going through some tutorials and rewriting some CLI utilities I'd written in Rust or Go into Nim (they're part of my job, so I can't share them).

I played with the language off and on through October and November, and decided that I'd do this year's [Advent of Code](https://adventofcode.com/) entirely in Nim as continued exercise in the language.

## Working through Advent of Code

I participated in 2018's AoC a bit (only finished 11 stars), and wanted to participate more this year. As of writing this, I'm definitely behind - only 18 stars of an available 42 - but I've skipped a handful of parts that just didn't sound interesting to me. You can find my work for this year's AoC [on my GitHub](https://github.com/celeo/advent2019).

### Notable good parts

* Simple syntax
* Good module documentation
* Easy access to [functional programming](https://en.wikipedia.org/wiki/Functional_programming) functions like `map`, `filter`, and `fold` through the [sequtils module](https://nim-lang.github.io/Nim/sequtils.html)
* Easy file I/O (important for reading input for every part)
* Clear separation between mutable and immutable variables/parameters helps prevent bugs
* Being able, though not _required_, to use symbols as proc names

### Things that I don't like

* Having to create a `new<name>` proc for simpler object creation syntax is annoying. Example:

```nim
type
  Point = object
    x: int
    y: int
    steps: int

block:
  let
    x = 1
    y = 2
    # without a proc to create the object, each variable has to be explicitly assigned
    p = Point(x: x, y: y)

# helper proc
proc newPoint(x, y, steps: int): Point =
  Point(x: x, y: y, steps: steps)

block:
  let
    x = 1
    y = 2
    # now this object can be created via the proc
    p = newPoint(x, y)
```

* Writing assertions via `doAssert` or `assert` - they properly throw a runtime exception if the assertion fails, but don't print out any information about the assertion like in Java, JavaScript, Rust, etc. It's not a good developer UX to have to _print both sides_ of an assertion _before the assertion_ that's failing, _for every assertion_, just to debug
* Compiler errors can be hard to understand and point to parts of the code that aren't actually the problem

## What I think now

Overall, I really like Nim, and look forward to its future as development continues. There are a number of things that are missing, like a good mocking library, a eslint/gofmt/rustfmt-style formatting tool, better assertions, better compiler error messages, etc., but the language lets me get to creating a solution for a problem without hassle, just like Python does. One of my biggest critiques of Rust is that I don't find it suitable for quick-and-dirty problem solving because of the time buy-in that all new projects take to set up (cargo is great, but for example, adding JSON support to a project takes like 3 minutes).

On Dec 20, the Nim team put out the [2019 Nim Community Survey](https://nim-lang.org/blog/2019/12/20/community-survey-2019.html), which is a great opportunity for developers to give their feedback on the language.

I'm planning on continuing using Nim at home and at work for all scripting and quick solutions I need, as well as looking into more complex usages, like web servers and the compile-to-JavaScript feature.

## Learning resources

Here are a handful of resources that will help to learn the language and ecosystem.

* [Main website](https://nim-lang.org/)
* [Module docs](https://nim-lang.org/docs/lib.html)
* [Manual](https://nim-lang.org/docs/manual.html)
* [Style guide](https://nim-lang.org/docs/nep1.html)
* [Getting started](https://nim-by-example.github.io)
* [Ground-up tutorial](https://narimiran.github.io/nim-basics/)
* [Nim book](https://www.oreilly.com/library/view/nim-in-action/9781617293436/)
