# HTTP/2 performance

## HTTP/1.1
* persistent connections (reuse existing connections)
  * connections are costly to create
* headers

## Performance workarounds
* multiple connections
* CDN
* concatenation / splitting
* compression / minification
* caching
* sharding

## HTTP/2
* evolution of SPDY
* same semantics as HTTP/1
* binary format
* uses a single connection
* HTTPS mandatory for browsers
* does not affect WebSockets

## Framing
Allows to avoid head of line blocking where one requests takes a long time to be processed, slowing down all the next requests.
HTTP/2 allows real multiplexing

HTTP/2 defines multiple kinds of frames
* 

## Header compression
HTTP/2 compresses the headers

## Push
Server side can push data to the client (server initiated push)

## Multi-threading vs streaming
With HTTP/2 we want to do concurrency, not multi-threading.
To make the best usage of resources and CPU, we should use streaming.

The thing is to write asynchronous and non-blocking code.

Reactor pattern:
* dispatch events in an efficient manner
* event loop
* single thread

Benefits:
* easier to scale
* simple concurrency model
* no context switching, better use of CPU cache

## Multi-core
Most efficient is to apply the event loop pattern on multiple cores