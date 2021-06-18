---
title: Implementing an IRC server from scratch, part 1
date: 2021-06-18 12:23:28
tags:
- programming
---

Let's implement [RFC1459](https://datatracker.ietf.org/doc/html/rfc1459).

<!-- more -->

With all of the buzz around [Freenode's demise](https://www.reddit.com/r/linux/comments/o0263h/all_freenode_channels_and_users_gone/), [Libera.Chat's rise](https://libera.chat/), and the renewed (or popularized) interest in IRC, I think it'll be fun and engaging to implement an IRC server from scratch with RFC1459 to follow.

Now, I'm not going to implement _everything_. Probably. My goal is to implement a _single_ server that clients can connect to, join channels, chat, privmsg each other, etc., but I'm not really looking to make server-to-server communication or NickServ/ChanServ. Since I'm currently using [HexChat](https://hexchat.github.io/) to connec to IRC servers right now, if I can make use of that and maybe [irssi](https://irssi.org/) to connect to my "simple" implementation of IRC and chat between the clients, I'll call it a success!

## Tech

I spent a bit of time deciding what tech I wanted to use. I could certainly do this with Python. I could use Rust; the level of complexity in this program would probably lend itself well to Rust's great tooling. I could go back to Go, as its exemplary concurrency support would likely make it simple. But, considering I've been playing with Elixir over the past few months, and Elixir _really_ makes working with servers/services very straightforward, and I really enjoy it as a language, it's a natural fit.

## Getting started

The first thing I wanted to do was have a read over the RFC for IRC and take notes. The first, _first_ thing I wanted to do was find a way to read RFCs that doesn't melt my eyes. I poked around the web a bit, and found <https://www.rfcreader.com/>, which makes reading RFCs much nicer, as well as including a handful of other useful features (I'm not making use of any of the features that require log-in, but they seem nice).

Once I had a read through and [made a bunch of notes](https://github.com/Celeo/irc_playground/blob/master/notes.md), I needed to get familiar with what this will look like in Elixir. I used a few resources for this:

1. the official [Task and gen_tcp](https://elixir-lang.org/getting-started/mix-otp/task-and-gen-tcp.html) tutorial
1. the official [GenServer](https://elixir-lang.org/getting-started/mix-otp/genserver.html), [supervisor](https://elixir-lang.org/getting-started/mix-otp/supervisor-and-application.html), and [dynamic supervisor](https://elixir-lang.org/getting-started/mix-otp/dynamic-supervisor.html) tutorials
1. the docs for [GenServer](https://hexdocs.pm/elixir/GenServer.html), [supervisor](https://hexdocs.pm/elixir/Supervisor.html#content), and [dynamic supervisor](https://hexdocs.pm/elixir/DynamicSupervisor.html#content)
1. info on the differences between GenServer, Task, etc. [1](https://www.reddit.com/r/elixir/comments/mcnl65/difference_between_genserver_supervisor_task_and/), [2](https://elixirforum.com/t/tasks-vs-genserver/27864)
1. info on handling TCP connections, [1](http://www.robgolding.com/blog/2019/05/21/tcp-genserver-elixir/), [2](https://medium.com/blackode/quick-easy-tcp-genserver-with-elixir-and-erlang-10189b25e221), [3](https://stackoverflow.com/questions/40973079/managing-multiple-tcp-connections-in-active-mode-from-one-genserver)
1. how to stop a GenServer, [1](https://elixirforum.com/t/stopping-child-genserver-in-dynamicsupervisor-gracefully-without-restarting-it/17741/3), [2](https://alexcastano.com/how-to-stop-a-genserver-in-elixir/)

## Writing code

My first goal was to follow that first linked tutorial and implement a TCP server. Starting a new Elixir project is simple with `mix new [name]`. I didn't need any dependencies, so I could get right to following the tutorial. I implemented the tutorial in [5ac9f...](https://github.com/Celeo/simple_irc_server/commit/5ac9f8e365f395de99a7f6b8c272aaff539e0ea4) and [c804c...](https://github.com/Celeo/simple_irc_server/commit/c804c07b54076b051bb3349fee06a2b33899fad6).

After that, I knew that I wanted to move from `Tasks` to `GenServers`, as I want to be able to communicate with these "processes", as well as have them hold state. This involved swapping `Tasks` and the `Task Supervisor` for `GenServers` and a `Dynamic Supervisor`. I did this work in [2cafd...](https://github.com/Celeo/simple_irc_server/commit/2cafd8c81947f4df098fcd16495a69050bf804bb). I'll break down what the project currently looks like.

### Pieces

First, we need an `Application` to launch and start the supervisor tree. [app.ex](https://github.com/Celeo/simple_irc_server/blob/2cafd8c81947f4df098fcd16495a69050bf804bb/lib/app.ex) does this. Implementing an application is very simple - you need `use Application` in your module, as well as to implement `start(type, args)`.

Here, I'm getting the port to run on from the "PORT" environment variable with a fallback to a static value:

```elixir
port = String.to_integer(System.get_env("PORT") || @default_port)
```

The next bit here sets up the processes that will be entered into the supervisor tree:

```elixir
children = [
  {DynamicSupervisor, name: IRC.DynamicSupervisor, strategy: :one_for_one},
  Supervisor.child_spec({Task, fn -> IRC.Listener.accept(port) end}, restart: :permanent),
  IRC.Server
]
```

The first item in the list is a `Dynamic Supervisor` with a name. This will hold connections with clients. The next is a `Task` that runs a function (getting the port) to wait for connections from clients. This task will always be restarted if it fails (`restart: :permanent`). This process doesn't have any state, so it doesn't need to be a `GenServer`. Finally, a module referencing a server.

That server is in [server.ex](https://github.com/Celeo/simple_irc_server/blob/2cafd8c81947f4df098fcd16495a69050bf804bb/lib/server.ex) and is _very_ simple right now, just implementing the minimums of a `GenServer` module.

The listener in [listener.ex](https://github.com/Celeo/simple_irc_server/blob/2cafd8c81947f4df098fcd16495a69050bf804bb/lib/listener.ex) has a little more to it. Remember that this is ran via a `Task`. The `accept` function here sets up the TCP server:

```elixir_
@listen_args [:binary, packet: :line, active: true, reuseaddr: true]
# -snip-
  {:ok, socket} = :gen_tcp.listen(port, @listen_args)
```

and then calls `loop_acceptor` to loop indefinitely to take new connections and fire off a new `GenServer` process to hold that connection, which is registered to the `Dynamic Supervisor` in the application supervisor tree:

```elixir
{:ok, client} = :gen_tcp.accept(socket)
{:ok, pid} = DynamicSupervisor.start_child(IRC.DynamicSupervisor, {IRC.ClientConnection, {client}})
:ok = :gen_tcp.controlling_process(client, pid)
```

This code (1) waits for and takes a new connection, (2) creates a new GenServer process for that client, and (3) then assigns the controlling pid for the socket connection to that new process. Not too complicated, and it's not too dissimilar from the `Task`-oriented code directly from the tutorial.

Finally, in [client_connection.ex](https://github.com/Celeo/simple_irc_server/blob/2cafd8c81947f4df098fcd16495a69050bf804bb/lib/client_connection.ex), the first interesting line is the second line of the file:

```elixir
use GenServer, restart: :transient
```

Here, we're declaring this module as a `GenServer`, but passing an argument to this (which is passed to the child\_spec) that tells the `Dynamic Supervisor` _not_ to restart connections if they fail. I don't want the _server_ to attempt to reconnect to clients if clients disconnect; I want the clients to try to reconnect to the server, and the server to just drop the connection if something disconnects it.

I'll copy the other interesting parts of this file here:

```elixir
# Normal messages from the client.
@impl true
def handle_info({:tcp, _socket, message}, state) do
  IO.inspect(message)
  {:noreply, state}
end

# Socket closed
@impl true
def handle_info({:tcp_closed, _socket}, state) do
  Logger.info("Socket has been closed")
  {:stop, :normal, state}
end

# Socket had an error
@impl true
def handle_info({:tcp_error, _socket, reason}, state) do
  Logger.info("Connection closed: #{reason}")
  {:noreply, state}
end

# Other event
@impl true
def handle_info(_event, state) do
  {:noreply, state}
end
```

I've commented before each method implementing `handle_info`. Because of the `active: true` argument I'm passing to `:gen_tcp.listen` in "listener.ex", the GenServer will get packages from the client in its `handle_info` callback. This isn't the only way to do this; you could use `active: false` and then explicitly poll the socket for data. For one tutorial I read on implementing a Redis client, this is fine, as data will only come back after going on. For an IRC server though, communication is driven by clients, so we need to be able to receive data at the same time as processing other things.

## Next steps

What's next? Well, probably command parsing, at least the _very_ basic commands that are involved with connecting and sending messages. Then I'll need to start the command framework for the server, where it can receive commands and process them accordingly. Somewhere in there, I'll be creating a large data structure with numeric reply codes, strings, and messages. Regardless of what I go for next, I'll be translating a lot of information from the RFC into code. I could look for Elixir libraries that implement these details themselves and import from them, and I would if were doing this project as a way to get a server running and _then_ do something with it. Here though, the entire goal is to make the server from "scratch" and so I won't be importing any such libraries.
