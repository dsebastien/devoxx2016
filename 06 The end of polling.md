# The end of polling

## Links
* Intro
  * https://www.html5rocks.com/en/tutorials/eventsource/basics/
  * https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events
  * https://en.wikipedia.org/wiki/Server-sent_events
  * https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events
* Server-Sent Events
  * Spec: https://www.w3.org/TR/eventsource/
  * EventSource polyfill: https://github.com/Yaffle/EventSource
* JSON Patch
  * RFC: https://tools.ietf.org/html/rfc6902
  * JS implementation: https://www.npmjs.com/package/fast-json-patch
* https://www.npmjs.com/package/observable-event-source
  * http://stackoverflow.com/questions/36827270/creating-an-rxjs-observable-from-a-server-sent-eventsource
* TypeScript typings: https://github.com/yankee42/typescript-server-sent-events
* Misc
  * AngularJS examples
    * https://alots.wordpress.com/2014/06/02/play-server-sent-events-and-angularjs/
    * https://github.com/mastbaum/tiny-angularjs-sse-example
  * https://hpbn.co/server-sent-events-sse/
  * SASS for SSEs: http://streamdata.io/

## About
Transforming REST APIs into Data Streaming APIs.
Problem with refresh button: waiting, loading, busy, please wait, etc

To keep user engaged we need animation, life, real-time.

## How
* (long) polling
* web sockets
* server-sent events

## Polling
Make consecutive requests, retrieves empty responses, retrieves the same data multiple times, misses data, ...
Clearly not ideal.

## Web Sockets
* W3C spec: https://www.w3.org/TR/2009/WD-websockets-20091029/
* Bi-directional pipe to send events between client and server
* Handles any data type
* TCP based binary protocol: https://tools.ietf.org/html/rfc6455
* Message format not defined
* Connection hang-ups not defined (handle as you like)

## Server-sent events (SSEs)
* WhatWG spec: https://html.spec.whatwg.org/multipage/comms.html#server-sent-events
* W3C spec: https://www.w3.org/TR/eventsource/
* Uni-directional pipe (server to client)
* Only handles text
* HTTP protocol: hence compatible with most infrastructure pieces (load balancers, firewalls, etc)
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

Example: http://blog.samshull.com/2010/10/ajax-push-in-ios-safari-and-chrome-with.html

## SSE retry
Add the retry field on the message to state that SSE should try to send it multiple times
Using ids we can also 

## Browser support
Supported by modern Web browsers. Not supported by IE11, still under consideration on MS Edge: https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer/suggestions/6263825-server-sent-events-eventsource
Support matrix: http://caniuse.com/#feat=eventsource
There's a polyfill we can use: https://github.com/Yaffle/EventSource

## Performances
Faster than Web sockets

## Streamdata.io
Free SASS service: "proxy as a service".

* streaming based on
* Server-Sent events
* dynamic cache
* incremental updates

They use JSON-PATCH (RFC 6902)

## Security
SSEs have a better security stance for now, as there are known issues with Web Sockets. Since SSEs rely on HTTP, all usual mechanisms (e.g., cookies) can still be relied upon, so it's "easy" to handle authentication/authorization.

## Headers issue
In order to route the requests to the Information System, the proxy must be able to forward the headers of the requests (port number, OAuth authentication, …). In addition, to secure the transactions going through the proxy, we will setup an authentication mechanism based on security tokens that will also be routed to the Proxy.
Yet, the current implementations of EventSource object in JavaScript (base object for SSE communication) does not allow to override the headers. This problem does not exist with native SSE libraries (Java or ObjectiveC).

One workaround is to pass values through query params.

Related article: http://streamdata.io/blog/push-sse-vs-websockets/

## Disconnection detection
When a client is connected to the server via SSE, the server is not notified if the client disconnects. Disconnection will be detected by the server only when trying to send data to the client, and getting an error report mentioning that the connection was lost.
This is a problem in our case because the server performs regular pollings to the information system, and we want to stop this polling at the earliest to avoid inducing unnecessary load on the IS when no client is connected. In some cases, if the data source is not very dynamic, it may take several pollings before the data is changed, so that the server tries to send a diff to client.
This is problematic, so we considered different solutions which will be presented next.

To address this issue, we can set up a heartbeat mechanism sent by the server at regular intervals. With SSE, if you send a blank character, it does not induce network overload or additional processing at the client level, and simply will detect disconnection without waiting for data to be sent.

Related article: http://streamdata.io/blog/push-sse-vs-websockets/