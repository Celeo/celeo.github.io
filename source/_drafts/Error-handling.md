---
title: Error handling
date: 2019-06-08 20:23:28
tags:
---

Some thoughts on error handling in different programming languages.

<!-- more -->

## Disclaimer

I'm certainly not an expert on the subject or on all of the languages that I'll be writing about here. What's below is my opinion.

## Outline

1. Intro
1. Languages
    1. Java
    1. Python
    1. JavaScript
    1. Go
    1. Rust

## Intro

Error handling is an important part of any programming language. We can't always prepare for hardware failure, or spontaneous data corruption, or a power outage, but we can and must plan for more commonplace operations that can fail, anything from a string not being able to be turned into a number, a socket failing to lock a port, or a user entering invalid data. How each language handles errors (note that I'll be using "errors" and "exceptions" interchangeably) is a big part of how the language feels to use.

## Languages

I'll be writing about error handling in some of the languages that I'm most familiar with. Some of these languages feature error handling that's basically the same as others in this list.

### Java

The enterprise language. I don't write any Java for my personal projects, as I don't find it a particularly "engaging" language, but it's what I mostly use at work. Specifically, Java has "*exceptions*", which are thrown by functions. Java specifies two main types of exceptions: checked and unchecked. *Checked* exceptions must be handled in the code, either by re-throwing the exception to higher in the callstack, or by catching it with a `try-catch` block. *Unchecked* exceptions do not *have* to be explicitly handled - there's no compile-time enforcement of handling these errors. As a result of these exceptions being more hidden, they're often what I've seen giving developers the most problems - mostly often `NullPointerExceptions`, followed by `ArrayOutOfBoundsExceptions`. Whether or not there should even *be* `NullPointerExceptions` in a language is a topic for an entirely separate article. Unchecked exceptions can be caught with the same `try-catch` blocks as checked exceptions, but it's up to the developer to remember to code defensibly to prevent these exceptions from happening or add the catch blocks to handle them appropriately; these bugs won't be found until someone actually goes and runs the code.

```java
private static void foo(String s) throws SomeCheckedException {
    ...
}

public static void main(String[] args) {
    try {
        // Even though I'm only handling the `SomeCheckedException` here explicitly,
        // this is still highly likely to throw an unchecked exception.
        foo(args[0]);
    } catch (SomeCheckedException e) {
        System.err.println("Exception!");
    }
}
```

### Python

Python is similar to Java in how to handles "errors", with a couple of differences:

* They're called "errors", not "exceptions"
* No compile-time enforcement of handling errors (obviously)
* Syntax (`try/except` / `raise`)

```python
def foo(s):
    ...

def main():
    try:
        foo('a')
    except Exception as e:
        print('Exception!')
```

### JavaScript

Another similar syntax.

```javascript
function foo(s) {
    ...
}

function main() {
    try {
        foo('a')
    } catch (error) {
        console.log('Exception!')
    }
}
```

### Go

Multiple return values.

TODO

### Rust

Results.

TODO
