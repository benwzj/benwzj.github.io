---
layout: post
title: WebSocket Introduction
date: 2023-10-21
category: Protocol
tags: WebSocket Http Tcp
---

## What is WebSocket

WebSocket is a computer communications protocol, providing full-duplex communication channels over a single TCP connection.

WebSocket is distinct from HTTP. Both protocols are located at layer 5 in the OSI model and depend on TCP at layer 4.

Although they are different, RFC 6455 states that WebSocket "is designed to work over HTTP ports 443 and 80 as well as to support HTTP proxies and intermediaries," thus making it compatible with the HTTP protocol.

To achieve compatibility, the WebSocket handshake uses the HTTP Upgrade header to change from the HTTP protocol to the WebSocket protocol.

The WebSocket protocol enables interaction between a web browser (or other client application) and a web server with lower overhead than half-duplex alternatives such as HTTP polling, facilitating real-time data transfer from and to the server. 
This is made possible by providing a standardized way for the server to send content to the client without being first requested by the client.

## Websocket Test

### Websocket Client Testers

- Testing using browser through[piesocket](https://www.piesocket.com/websocket-tester) or [socketsbay](https://socketsbay.com/test-websockets)
- Using `zsh` test: **wscat** 

### WebSocket server

- websocket-echo.com is a simple echo websocket server. support ws and wss.
```
ws://websocket-echo.com
wss://websocket-echo.com
```
- Free Chatroom
I apply a free WebScoket chatroom in socketsbay. 
```
wss://socketsbay.com/wss/v2/[ChannelId]/[ApiKey]/
```
You can use wscat to connect to the chatroom.
```
wscat -c wss://socketsbay.com/wss/v2/1/1f437d249c2ca2eb3c415b9f92df92c8/
```

## Uing AWS WebSocket API Gateway Create Group Chat application
This example will show how to create Group Chat applicatin using AWS WebSocket API Gateway.

### Overview
1, Clients join the chat room as they connect to the WebSocket API.
2, The backend can send messages to specific users via a callback URL that is provided after the user is connected to the WebSocket API.
3, Users can send messages to the room.
4, Disconnected clients are removed from the chat room.

