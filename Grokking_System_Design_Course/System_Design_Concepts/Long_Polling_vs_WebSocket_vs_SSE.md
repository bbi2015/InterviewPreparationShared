# Long-Polling vs WebSockets vs Server-Sent Events

Long-Polling, WebSockets, and Server-Sent Events are popular communication protocols between a client like a web browser and a web server. First, let’s start with understanding what a standard HTTP web request looks like. Following are a sequence of events for regular HTTP request:

- The client opens a connection and requests data from the server.
- The server calculates the response.
- The server sends the response back to the client on the opened request.

<img width="800" alt="Screenshot 2023-01-11 at 5 01 03 PM" src="https://user-images.githubusercontent.com/33375484/211795688-4fb78ce5-863a-4023-9811-8b7b3b69a662.png">

## Ajax Polling

Polling is a standard technique used by the vast majority of AJAX applications. The basic idea is that the client repeatedly polls (or requests) a server for data. The client makes a request and waits for the server to respond with data. If no data is available, an empty response is returned.

- The client opens a connection and requests data from the server using regular HTTP.
- The requested webpage sends requests to the server at regular intervals (e.g., 0.5 seconds).
- The server calculates the response and sends it back, just like regular HTTP traffic.
- The client repeats the above three steps periodically to get updates from the server.

The problem with Polling is that the client has to keep asking the server for any new data. As a result, a lot of responses are empty, creating HTTP overhead.

<img width="795" alt="Screenshot 2023-01-11 at 5 01 14 PM" src="https://user-images.githubusercontent.com/33375484/211795717-65112083-3b3a-468e-936e-ab2143f7de70.png">

## HTTP Long-Polling

This is a variation of the traditional polling technique that allows the server to push information to a client whenever the data is available. With Long-Polling, the client requests information from the server exactly as in normal polling, but with the expectation that the server may not respond immediately. That’s why this technique is sometimes referred to as a “Hanging GET”.

- If the server does not have any data available for the client, instead of sending an empty response, the server holds the request and waits until some data becomes available.
- Once the data becomes available, a full response is sent to the client. The client then immediately re-request information from the server so that the server will almost always have an available waiting request that it can use to deliver data in response to an event.

The basic life cycle of an application using HTTP Long-Polling is as follows:

- The client makes an initial request using regular HTTP and then waits for a response.
- The server delays its response until an update is available or a timeout has occurred.
- When an update is available, the server sends a full response to the client.
- The client typically sends a new long-poll request, either immediately upon receiving a response or after a pause to allow an acceptable latency period.
- Each Long-Poll request has a timeout. The client has to reconnect periodically after the connection is closed due to timeouts.

<img width="754" alt="Screenshot 2023-01-11 at 5 01 23 PM" src="https://user-images.githubusercontent.com/33375484/211795738-749e4a9c-72b5-4dd8-a82e-b6a9acec1b35.png">

## WebSockets

WebSocket provides Full duplex communication channels over a single TCP connection. It provides a persistent connection between a client and a server that both parties can use to start sending data at any time. The client establishes a WebSocket connection through a process known as the WebSocket handshake. If the process succeeds, then the server and client can exchange data in both directions at any time. The WebSocket protocol enables communication between a client and a server with lower overheads, facilitating real-time data transfer from and to the server. This is made possible by providing a standardized way for the server to send content to the browser without being asked by the client and allowing for messages to be passed back and forth while keeping the connection open. In this way, a two-way (bi-directional) ongoing conversation can take place between a client and a server.

<img width="690" alt="Screenshot 2023-01-11 at 5 01 32 PM" src="https://user-images.githubusercontent.com/33375484/211795765-f7939604-bee7-4e0c-8163-203c844c0a2e.png">

## Server-Sent Events (SSEs)

Under SSEs the client establishes a persistent and long-term connection with the server. The server uses this connection to send data to a client. If the client wants to send data to the server, it would require the use of another technology/protocol to do so.

- Client requests data from a server using regular HTTP.
- The requested webpage opens a connection to the server.
- The server sends the data to the client whenever there’s new information available.

SSEs are best when we need real-time traffic from the server to the client or if the server is generating data in a loop and will be sending multiple events to the client.

<img width="696" alt="Screenshot 2023-01-11 at 5 01 39 PM" src="https://user-images.githubusercontent.com/33375484/211795792-d2ec073c-cb53-41d7-9b46-9ad45729d522.png">
