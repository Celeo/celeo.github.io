---
title: Unit testing in Nim
date: 2020-01-04 16:43:58
tags:
- programming
- nim
---

What I've learned by writing unit tests in [Nim](https://nim-lang.org).

<!-- more -->

## Outline

1. Initial attempts
1. Implementing `expect`
1. Finding `check`
1. Testing an async proc
1. Conclusion

## Initial attempts

When a developer creates a new library project, [nimble](https://github.com/nim-lang/nimble) creates a project scaffolding that includes a basic unit test:

```nim
# This is just an example to get you started. You may wish to put all of your
# tests into a single file, or separate them into multiple `test1`, `test2`
# etc. files (better names are recommended, just make sure the name starts with
# the letter 't').
#
# To run these tests, simply execute `nimble test`.

import unittest

import Downloads
test "can add":
  check add(5, 5) == 10
```

From this, I can see that the `test` word along with a string description is used to separate tests. Running `nimble test` runs them and gives an output showing the passing test. Makes sense.

## Implementing `expect`

I expanded on this to write a chunk of tests, and everything pretty much went well. Unfortunately, whenever a test I wrote would fail due to an assertion throwing an exception, I didn't get information on _why_ the assertion was failing, just that it was. I was used to [Jest's](https://jestjs.io) fantastic output, telling me what the parts of the assertion where at the time of evaluation.

```
/home/arch/Downloads/tests/test1.nim(13) test1
/home/arch/.choosenim/toolchains/nim-1.0.4/lib/system/assertions.nim(27) failedAssertImpl
/home/arch/.choosenim/toolchains/nim-1.0.4/lib/system/assertions.nim(20) raiseAssert
/home/arch/.choosenim/toolchains/nim-1.0.4/lib/system/fatal.nim(39) sysFatal

Unhandled exception: /home/arch/Downloads/tests/test1.nim(13, 10) `add(5, 5) == expected`  [AssertionError]
[FAILED] can add
Error: execution of an external program failed: '/home/arch/Downloads/tests/test1 '
       Tip: 1 messages have been suppressed, use --verbose to show them.
     Error: Execution failed with exit code 1
        ... Command: "/home/arch/.nimble/bin/nim" c --noNimblePath -d:NimblePkgVersion=0.1.0 "-r" "--path:."  "/home/arch/Downloads/tests/test1"
```

I went and wrote a [library](https://github.com/celeo/expect-nim) to improve on the assertions, so I'd know what each side where, in order to help me debug and fix tests (or the code). I didn't _want_ to have to do this, but I couldn't get information from the assertion, and this was definitely better than `echod`'ing every variable before asserting.

```
/home/arch/Downloads/tests/test1.nim(15) test1
/home/arch/.nimble/pkgs/expect-0.3.1/expect.nim(43) expectEqual

Unhandled exception:
===== Expect Error =====
Left: 10, right: 11
Checked for: equal
========================
 [ExpectError]
[FAILED] can add
```

## Finding `check`

I was diving through the (rather hard to read) `unittest` module, and found [`check`](https://nim-lang.org/docs/unittest.html#check.m%2Cuntyped):

```
/home/arch/Downloads/tests/test1.nim(14, 18): Check failed: add(5, 5) == expected
add(5, 5) was 10
expected was 11
[FAILED] can add
```

Fantastic!

As a result, I've archived my expect library with a note to use `check` instead. Problem solved.

## Testing an async proc

I wanted to drop this bit in, as I couldn't really find a clear approach. It turns out that writing a test for an async proc is no different than _calling_ that proc in a normal context - either use an `async` proc, or something like [`waitFor`](https://nim-lang.org/docs/asyncdispatch.html#waitFor%2CFuture%5BT%5D):

```nim
import unittest, asyncdispatch

proc doSomething(): Future[string] {.async.} =
  result = "foobar"

test "with async proc":
  proc doTest() {.async.} =
    let val = await doSomething()
    check val == "foobar"

  waitFor(doTest())

test "witout async proc":
  let val = waitFor(doSomething())
  check val == "foobar"
```

I ended up spending **hours** trying to figure out how to make this pattern happen, as I was getting very obscure error messages during runtime about "no handles or timers registered". Turns out this was an issue with me not setting up a stream correctly. For reference, here's that information.

If your code does this:

```nim
import
  asyncdispatch,
  httpclient

proc convertAsync*(response: AsyncResponse): Future[JsonNode] {.async.} =
  let body = await response.body()
  # ...
```

then your test needs to look like this:

```nim
import
  asyncdispatch,
  httpclient,
  streams

proc getAsyncResponse(): AsyncResponse =
  echo "getAsyncResponse()"
  result = AsyncResponse()
  # ...
  result.bodyStream = newFutureStream[string]()
  waitFor result.bodyStream.write("Hello world")
  result.bodyStream.complete()
```

I was missing the `complete` call, which apparently (?) caused the future to not be registered on the global dispatcher.

## Conclusion

Writing unit tests in Nim is very simple to get started, thanks to the syntax of the templates in `unittest` and the scaffolding that's created for the developer by `nimble` when creating a new library project.

The process gets more difficult, however, when the developer needs to move into things like actually seeing the values in an assertion and testing asynchronous procedures. Simple lesson: use `check`, not `assert` or `doAssert`. And, as long as you make sure that your procedures are correctly written, you won't have to muck through a ton of obscure error messages that don't do anything to help you through debugging. Tests are used to make sure that code is written correctly; unfortunately in this instance, I had to make sure my code was working correctly before I could run a test.
