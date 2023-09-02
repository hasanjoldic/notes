# WebSocket API

The WebSocket API makes it possible to open a two-way interactive communication session __between the user's browser and a server__. With this API, you can send messages to a server and receive event-driven responses without having to poll the server for a reply.

## Writing WebSocket client applications

In order to communicate using the WebSocket protocol, you need to create a `WebSocket` object; this will automatically attempt to open the connection to the server.

```js
webSocket = new WebSocket(url, protocols);
```

`url`

  The URL to which to connect; this should be the URL to which the WebSocket server will respond. This should use the URL scheme `wss://`, although some software may allow you to use the insecure `ws://` for local connections.

`protocols` _Optional_

If an error occurs while attempting to connect, first a simple event with the name error is sent to the `WebSocket` object (thereby invoking its `onerror` handler), and then the `CloseEvent` is sent to the WebSocket object (thereby invoking its `onclose` handler) to indicate the reason for the connection's closing.

### Sending data to the server

```js
exampleSocket.send("Here's some text that the server is urgently awaiting!");
```

You can send data as a `string`, `Blob`, or `ArrayBuffer`.

```js
// Send text to all users through the server
function sendText() {
  // Construct a msg object containing the data the server needs to process the message from the chat client.
  const msg = {
    type: "message",
    text: document.getElementById("text").value,
    id: clientID,
    date: Date.now(),
  };

  // Send the msg object as a JSON-formatted string.
  exampleSocket.send(JSON.stringify(msg));

  // Blank the text input element, ready to receive the next line of text from the user.
  document.getElementById("text").value = "";
}
```

## Writing WebSocket servers

A WebSocket server is nothing more than an application listening on any port of a TCP server that follows a specific protocol.

### The WebSocket handshake

First, the server must listen for incoming socket connections using a standard TCP socket. Depending on your platform, this may be handled for you automatically. For example, let's assume that your server is listening on example.com, port 8000, and your socket server responds to GET requests at example.com/chat.

> Browsers generally require a secure connection for WebSockets.

The handshake is the "Web" in WebSockets. It's the bridge from HTTP to WebSockets. In the handshake, details of the connection are negotiated.

#### Client handshake request

Even though you're building a server, a client still has to start the WebSocket handshake process by contacting the server and requesting a WebSocket connection. So, you must know how to interpret the client's request. The client will send a pretty standard HTTP request with headers that looks like this (the HTTP version must be 1.1 or greater, and the method must be GET):

```bash
GET /chat HTTP/1.1
Host: example.com:8000
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

> Note: All browsers send an `Origin` header. You can use this header for security (checking for same origin, automatically allowing or denying, etc.) and send a `403 Forbidden` if you don't like what you see. __However, be warned that non-browser agents can send a faked Origin. Most applications reject requests without this header.__

#### Server handshake response

When the server receives the handshake request, it should send back a special response that indicates that the protocol will be changing from HTTP to WebSocket:

```bash
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

> Note: The server can send other headers like `Set-Cookie`, or ask for authentication or redirects via other status codes, before sending the reply handshake.

### Pings and Pongs: The Heartbeat of WebSockets

At any point after the handshake, either the client or the server can choose to send a ping to the other party. When the ping is received, the recipient must send back a pong as soon as possible. You can use this to make sure that the client is still connected.
