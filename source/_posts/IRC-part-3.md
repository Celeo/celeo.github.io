---
title: 'IRC, part 3'
date: 2021-07-02 19:05:43
tags:
- programming
- irc
---

I've made a good bit of progress, including figuring out things I noted in the last post.

<!-- more -->

## RFC differences

Now that I'm looking at the IRC RFC that modern clients & servers are using, things are making more sense and what I'm recording on the IRC client I'm using to connect to Libera.Chat is matching up with RFC 2812.

I still don't get how sending the user's nick back to them is documented in the semi-BNF for messages in the RFC document, but at least it seems consistent enough for me to model my implementation after. One of the goals of this project was to see how far I could get _just_ with the RFC, so this is an interesting experience, and I'm not discouraged by having to look at logs for existing client/server interaction.

## Code improvements

In the last article, I noted how I was confused about having to use `Task`s to wrap intra-`GenServer` communications. I didn't understand why I could make _some_ cross-process calls without a `Task` but others would give an error along the lines of "process cannot call itself". The issue was pretty simple, and I'll break down the code flow for you here so you don't have to go searching through the code.

1. The user (the person using a client) connects to the TCP server
1. They type a message and send it through their client (an actual IRC client or telnet client, doesn't matter)
1. The IRC server I'm writing gets that in the process for _that_ user's TCP socket, which I call [IRC.ClientConnection](https://github.com/Celeo/simple_irc_server/blob/fb7d7c1f5bc7be6ff5b9ca303a061462b4398330/lib/client_connection.ex), at [handle_info with a `{:tcp, socket, message}` shape](https://github.com/Celeo/simple_irc_server/blob/fb7d7c1f5bc7be6ff5b9ca303a061462b4398330/lib/client_connection.ex#L28)
1. That function does the parsing of the message and puts it in a tuple that's easier to work with down the line
1. The client process then [sends it](https://github.com/Celeo/simple_irc_server/blob/fb7d7c1f5bc7be6ff5b9ca303a061462b4398330/lib/client_connection.ex#L43) to the single server process
1. The server [gets the command](https://github.com/Celeo/simple_irc_server/blob/fb7d7c1f5bc7be6ff5b9ca303a061462b4398330/lib/server.ex#L33), [builds the module name for the command, and calls it](https://github.com/Celeo/simple_irc_server/blob/fb7d7c1f5bc7be6ff5b9ca303a061462b4398330/lib/server.ex#L37-L42), passing in the command information along with the calling client's state and the server's state
1. The individual module's function handles other processing and process communication

With this setup, I needed to wrap any process calls in the command modules with `Task.start(...)`, otherwise I'd get the aforementioned error. It's important to note that steps 3, 5, and 7 use **cross-process communication**, while step 6 uses a **direct function call**. Since most of these steps are already using cross-process communication, I didn't need to do anything to enable that. Since step 6 is using a direct function call, it's not taking advantage of _any_ of the cross-process communication features of the language and runtime.

To fix this, I simply wrapped that single call from the server to the command module with a `Task.start(...)` so that it would align with the rest of the code flow, and that worked. I was able to remove all `Task` process usage in the command modules and am able to use cross-process communication with any other process. Instead of running the command from the server process and wrapping messages _back_ to the server with a `Task`, I wrap the command processing in a `Task` so that it _is_ a process and can communicate with other processes like the rest of the application.
