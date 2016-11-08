# Reactive Streams in Java

## Links
* https://projectreactor.io
* https://github.com/reactor/lite-rx-api-hands-on.git
  * two branches (master & complete)
* https://github.com/bclozel/spring-reactive-university
* https://spring.io/blog/2016/04/19/understanding-reactive-types

## Changing the mindset
* everything should be written using non-blocking code/APIs
* Reactive Streams by itself is not enough: operators are needed to manipulate the streams
* nothing happens until something subscribes to the Publisher
* never reimplement Reactive Streams interfaces
* establish a common language (stream, non blocking, etc)

## Reactor Core 3
* natively designed on top of Reactive Streams
* lightweight API with 2 types: mono and flux
* native Java 8 support and optimisations
* 1MB jar
* ...
* Spring support

## Flux
* implements Reactive Streams Publisher
* 0 to n elements
* Operators: flux;map(...).zip(...).flatMap(...)

## Mono<T>
* implements Reactive Streams Publisher
* 0 to 1 element
* Operators: mono.then(...).otherwise(...)

## StepVerifier
* designed to test easily Reactive Streams Publishetrs
* carefully designed

```
StepVerifier.create(flux)
    .expectNext(...)
    ...
```

## Reactor 3 ecosystem
* Core
* Spring support
* Reactive Streams Commons -> Rx
* IO
* Addons

## Upcoming
* UI for troubleshooting reactive streams

## FlatMap
Use flatmap when dealing with elements that are themselves Publishers

## Exceptions.propagate
Returns a wrapped exception that will automatically be unwrapped by Reactor

## Schedulers.elastic
Create a dedicated thread pool

## Spring Reactive Web
...

## Server Sent Events
Nice in combination with stream oriented APIs (e.g., https://stream.gitter.io)

## Concurrency
* debugging concurrency issues remains hard
* tooling is important
  * with Reactor in Stack Trace mode (must have in dev), there's a collector of stack traces which provides useful information (class, line, etc)
* good reactive libraries matter
* requires defining additional code style rules in the teams