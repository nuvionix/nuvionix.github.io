---
title: "Socket Programming: Foundation of Network Communication"
date: 2026-11-26
draft: false
description: "Learn socket endpoints, TCP and UDP, client-server roles, and a simple local echo example."
categories: ["Python"]
tags: ["Python"]
readingTime: true
---

## What is a socket?

A socket is an endpoint through which programs send and receive data. In an Internet connection, an endpoint is commonly identified by an IP address and a port number, such as `127.0.0.1:5000`. The address identifies a host or interface; the port helps the operating system deliver data to the correct application.

Socket APIs allow processes on the same computer or on different machines to communicate using network protocols.

## Stream and datagram sockets

The two common transport choices are:

- **TCP (`SOCK_STREAM`):** connection-oriented and provides an ordered, reliable byte stream. Applications must still define message boundaries themselves.
- **UDP (`SOCK_DGRAM`):** sends independent datagrams without establishing a connection. It has lower protocol overhead, but delivery and ordering are not guaranteed.

Choose based on application requirements. Reliability, latency, message size, and tolerance for packet loss all matter.

## Client-server model

A server normally creates a socket, binds it to an address, listens for incoming TCP connections, and accepts a client. A client creates a socket and connects to the server. Once connected, both sides exchange bytes and close the connection when finished.

## A small TCP echo server

The following example is intended for local learning. It accepts one connection at a time and sends received bytes back to the client:

```python
import socket

HOST = "127.0.0.1"
PORT = 5000

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((HOST, PORT))
    server.listen()
    print(f"Listening on {HOST}:{PORT}")

    connection, address = server.accept()
    with connection:
        print("Connected by", address)
        while True:
            data = connection.recv(1024)
            if not data:
                break
            connection.sendall(data)
```

A matching client can connect to the same local address, send encoded text, and read the response. Use `sendall()` when the complete buffer must be sent; a single `send()` call is allowed to transmit fewer bytes than requested.

## Important details

TCP is a byte stream, not a message protocol. One `recv()` call does not necessarily correspond to one `send()` call. Real applications should define framing—for example, a fixed-size header, a length prefix, or a delimiter—and handle partial reads.

Also consider timeouts, connection failures, input validation, resource cleanup, and concurrency when designing a production server. Never expose a development server publicly without appropriate authentication, encryption, and network controls.

## Summary

Sockets provide the basic programming interface for network communication. Understanding addresses, ports, TCP/UDP differences, and the client-server lifecycle gives you a foundation for building chat systems, APIs, multiplayer applications, and distributed services.
