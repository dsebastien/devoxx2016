# The end of polling

## Links
* https://www.html5rocks.com/en/tutorials/eventsource/basics/
* https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events
* https://en.wikipedia.org/wiki/Server-sent_events
* https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events
* https://tools.ietf.org/html/rfc6902
* https://www.npmjs.com/package/observable-event-source
  * http://stackoverflow.com/questions/36827270/creating-an-rxjs-observable-from-a-server-sent-eventsource
* http://streamdata.io/
* TypeScript typings: https://github.com/yankee42/typescript-server-sent-events

## About
Transforming REST APIs into Data Streaming APIs.
Problem with refresh button: waiting, loading, busy, please wait, etc

To keep user engaged we need animation, life, real-time.

## How
* (long) polling
* web sockets
* server-sent events

## Polling
Make consecutive requests, retrieve empty responses, retrieve the same data multiple times, missing data, ...
Clearly not ideal.

## Web Sockets
* W3C spec
* Bi-directional pipe to send events between client and server
* Handles any data type
* binary protocol
* Message format not defined
* Connection hang-ups not defined (handle as you like)

Opening a Web socket

## Server-sent events (SSEs)
* W3C spec
* Uni-directional pipe (server to client)
* Only handles text
* HTTP protocol
* Message formate: any line of text starting with "data: " (can be JSON)

Other keywords:
* id
* event
* data
* ...

Opening a connection:
```
let eventSource = new EventSource("http://.../echo");
eventSource.onopen = () => { ... };
eventSource.onMessage = (e) => { 
    let message = e.data
};

eventSource.onError = (e) => { ... };
...
```

Possible to listen to specific event types:
```
eventSrc.addEventListener("eventType", (data) => {
    ...
});
```

## SSE retry
Add the retry field on the message to state that SSE should try to send it multiple times
Using ids we can also 

## Browser support
Not supported by IE11, still under consideration on MS Edge.
There's a polyfill we can use.

## Performances
Faster than Web sockets

## Streamdata.io
Free SASS service: "proxy as a service".

* streaming based on
* Server-Sent events
* dynamic cache
* incremental updates

They use JSON-PATCH (RFC 6902)

## Closing the event source
```
eventSrc.close();
```