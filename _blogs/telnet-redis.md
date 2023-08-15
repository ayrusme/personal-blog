---
title:  "Telnet and RESP"
date:   2023-08-16
permalink: redis-telnet
layout: blog_post
categories: ["redis","telnet","resp","databases"]
---
# Redis without the CLI (or the GUI)

While running some scripts to clear cache, a thought occured to me to experiment this mundane work. Every single time its the same story whenever i want to clear some keys from redis. It starts with consolidating the list of keys and then decide on whether to delete them with `DEL` or `UNLINK`. I know there is probably no way around the delete aspect, but I figured maybe I could try and issue the commands to redis directly without the help of the CLI. Born out of sheer laziness, I realised redis must be using a prootocol to accept commands. 

## Redis Serialization Protocol (RESP)

Redis uses a protocol called the [Redis Serialization Protocol (RESP)](https://redis.io/docs/reference/protocol-spec/), which is a simple text-based protocol that allows clients to send commands to the server and receive responses. The protocol is designed to be human-readable and easy to parse. When you use Telnet to connect to a Redis server, you're essentially opening a raw socket connection to the Redis server's port. You can then manually type in Redis commands following the RESP format. 

The basic structure of a Redis command is:

```bash
*<number of arguments>\r\n$<length of argument>\r\n<argument>\r\n
```

For example, the command SET key value in RESP format would look like:

```bash
*3\r\n$3\r\nSET\r\n$3\r\nkey\r\n$5\r\nvalue\r\n 
```

Here's a breakdown of how it works:

- `*3`: The asterisk followed by the number indicates the number of arguments in the command.
- `$3`: The dollar sign followed by the number indicates the length of the argument that follows.
- `\r\n`: Represents a newline character, indicating the end of a line.
- `SET`, `key`, `value`: These are the actual arguments of the Redis command.

When you type this manually using Telnet, you're essentially constructing the RESP-formatted command and sending it to the Redis server. The server processes your command and sends back a response in the same RESP format.

![results](assets/images/redis_telnet.png){:class="img-fluid"}

## Alternative approaches

Outside of `telnet`, any other tool capable of handling network connections and read/write data via TCP can help with this. One popular tool is `netcat`, which can also send and receive data. The only downside of using `netcat` for this operation is that `telnet` can be interactive, but `netcat` needs all the commands to be parsed and sent after establishing the connection. 

## Suggested Reading

[Redis Protocol](https://redis.io/docs/reference/protocol-spec) - This is the official spec for the redis protocol, would recommend reading the section `High-performance parser for the Redis protocol` if you're interested in writing your own redis client. 