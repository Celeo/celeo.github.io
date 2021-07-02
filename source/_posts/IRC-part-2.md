---
title: 'IRC, part 2'
date: 2021-07-01 22:43:04
tags:
- programming
- irc
---

In which I just discovered that RFC 2812 updates 1459 and the response format I've been playing with is wrong.

<!-- more -->

## 2812

First off, yeah, I just discoverd [RFC 2812](https://www.rfcreader.com/#rfc2812). At least section 6 contains this:

> Because of the small amount of changes in the client protocol since the publication of RFC 1459 [IRC], implementations that follow it are likely to be compliant with this protocol or to require a small amount of changes to reach compliance.

So that's nice.

## Progress

Not a whole lot, really. Most of the time I've put into the [code](https://github.com/Celeo/simple_irc_server) has been for working with Elixir and OTP rather than quickly implementing IRC concepts. I have an [implementation of the NICK command](https://github.com/Celeo/simple_irc_server/blob/22f867484817d2db45e7f4a1e2ad9bd939f916d0/lib/commands/nick.ex) that should cover the various conditions (new vs already-connected client & unused vs in-use nickname). I need to do USER next, but I took a shortcut and played around with WHO to make sure that two users could actually connect and have two different nicknames (as they're required to by the protocol). That worked, but then I discovered that the (albiet few) messages I've been sending haven't been in keeping with the messages I've actually received from Libera.Chat.

Hexchat has a nice feature where it'll log the raw IRC commands (incoming and outgoing) to a separate window that you can save to a file on your computer. It's a bit of a shame that this isn't _always_ logging to an in-memory buffer and the user has the option to save to a file, but it's not really a problem since I can connect to a server, then disconnect, then open that logging window, and then reconnect to the server to see the handshake process and all of the information that goes into that.

The commands I've been sending back to the client have looked like this:

> :server 436 :Nickname collision KILL

Which I thought, based on [section 5.2](https://www.rfcreader.com/#rfc2812_line2404) was accurate, but based on the commands I see in the IRC client I use to connect to an actual IRC server that's not correct. For example, this is what I get if I connect to Freenode with two telnet clients and send the same NICK command:

> :kodama.freenode.net 433 asdtwr asdtwr :Nickname overruled.

Note that I'm not actually using Freenode because it's _good_, but because it's easy to connect to and I'm not concerned about sending them bad commands.

Here, I've got the first bit (effectively) correct, I'm responding with `:<server name> <code>` but then the server here is including my nick twice. Based on [RFC 2812 section 2.3.1](https://www.rfcreader.com/#rfc2812_line252), I see where the "prefix" would _either_ be the servername or nick/user, but I don't see where _both_ are included.

Thankfully I now see the details for numeric replies 001-099 that I saw in my client logs for Libera, so that removes some confusion. Scanning through the other numeric replies, I see some that I haven't looked at so far.

For a note on Elixir & OTP, I'm having to do some work in `Task`s, as I thought I was able to send `cast` calls to other processes while "in" processing for a call/cast. They're separate processes, so I should be able to have them do work in parallel, right? Apparently that's _sort of_ true, because I can have a _task_ send the commands to another process, but not call it directly from, for example, a GenServer process. I doubt it really makes a different for the implementation, but it makes the code more complicated/busy.

## Next steps

A few things need to happen to move forward:

- I need to gain a better understanding of the IRC messaging format
- I need to improve the functions I'm using to send data to client TCP connections both to comply with the IRC standard and also so that those functions aren't annoying to use
- I'd like to figure out why I have to use `Task`s to send commands cross-process
