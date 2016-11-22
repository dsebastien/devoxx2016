# Reactive Web Applications with Spring 5

## Links
* https://www.youtube.com/watch?v=Cj4foJzPF8o

## Long running shift to Concurrency
CPUs are not getting much faster, we are only getting more and more cores.
For this reason, we must start to really care about concurrency to take advantage of those additional cores.

## Single thread and blocking
When you have a single thread, you can't block it.
When you have to perform a long running operation and you have no control over how long it will take to complete, you really don't want to block the entire server.
You need to make that asynchronous and to have a callback tell you when the waiting is over.

## Issues
We have lots of APIs in Java that are synchronous (i.e., blocking), and we usually write code in an imperative style.

## Async, non-blocking and reactive, .. why?
At the moment, we are using threads, we have thread pools. We are thus asynchronous, but we're not writing non-blocking code!

When we use a thread pool and allocate a thread for processing, we have to wonder how long each of those threads is being blocked. There's a cost associated with blocking.

There's a limit to how scalable we can get with multi-threaded blocking code: horizontal scaling limit.

With asynchronous & non-blocking code, we can do vertical scaling like NodeJS does.

This approach allows for better utilization of the hardware.

When approaching async and non-blocking, we have to deal with more and more events. Reactive programming helps.

## Could or should?
This approach is useful for scaling, but not mandatory if you don't have scaling issues.

## Servlet API issues
Servlet API input and output streams are blocking.

Servlet 3.0 async is still built on top of a blocking model. When reading and writing we're still using the same blocking APIs.

The threading model of the Servlet API is also an issue.

There is servlet 3.1 non-blocking IO API: can't combine that very well with the rest of the Servlet API (because many things are blocking) (e.g., request.getParameters -> blocking).

# Spring reactive
Not using servlet API, had to start from scratch.

No single best fluent (Java) async API...
What was missing was "push"-style operations on items as they become available from an active source (actual stream processing).

The stream API offers pull-based where we can pull items out of collections easily.
CompletableFuture is a nice continuation-style API that we can set the item on.

The issue with pull style is that when we pull data/results, we have to block while when the data is pushed to us then we can react when the data is available and we don't need to pull/wait while the data is not there.

## Reactive Streams
Specification included in JDK9 and "polyfilled" for now via third party libraries.

```
public interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}

public interface Subscriber<T> {
    void onSubscribe(Subscription sub);
    void onNext(T item);
    void onError(Throwable ex);
    void onComplete();
}

public interface Subscription {
    void request(long n);
    void cancel();
}
```

What's interesting with the above is the back-pressure support. Instead of the publisher pushing things to the subscriber at any time, the subscriber has the possibility to request items and the publisher can only push items if the subscriber is asking for them.

The reactive streams specification has been created by many large vendors in collaboration (e.g., Netflix, Twitter, Pivotal, ...) and is shared by many reactive streams APIs.

## Example of imperative and blocking style code

```
try {
    User user = userRepository.findById(id);
} catch(IOException ex) {
    // ...
}
...

interface UserRepository {
    User findById(Long id);
    List<User> findAll();
    void save(User user);
}
```

Those operations can block. The imperative programming model is simple but blocking.

## Example of asynchronous and non blocking alternative
```
Publisher<User> publisher = repository.findById(id);
...

interface UserRepository {
    Publisher<User> findById(Long id);
    Publisher<User> findAll();
    Publisher<Void> save(Uxer user);
}
```

findById should return at most 1 result, while findAll might return n results. Although the Provider generic type is the same in both cases. The difference with findAll is the number of times the onNext method will be called before onComplete is called.

With this approach, when we use the repository we don't get back the User object immediately, we get back a producer, meaning that we should get the data at some point later in time. This makes our code asynchronous and non blocking.

Now we need to use composition to explain what should happen once we get the user object back.

## Reactive streams APIs
Instead of dealing directly with the callbacks as we did in the previous example, we should use a Reactive Streams API like Reactor or RxJava.
Those provide higher level abstractions based on the reactive streams specification (e.g., Observable in the case of RxJava, Mono and Flux in the case of Reactor).

In addition, those libraries provide many operators to process the data asynchronously (e.g., map, reduce, filter, flatMap, debounce, ...).

## Example of asycnrhonous, non blocking and functional code
Example based on the Reactor library:

```
repository.findAll()
    .filter(user -> user.getName().matches("J.*"))
    .map(user -> "User" + user.getName())
    .log()
    .subscribe(user -> {});

...
interface UserRepository {
    Mono<User> findById(Long id);
    Flux<User> findAll();
    Mono<Void> save(User user);
}
```

## Spring Framework 5

### Reactive HTTP Server abstraction
```
public interface HttpHandler {
    Mono<Void> handle(ServerHttpRequest req, ServerHttpResponse res);
}
```

Asynchronous and non-blocking by design.

Compared to the Servlet API:
* Mono<Void> instead of void as return type
* Request and Response types not those of Servlet API (need to read & write in a non-blocking way)

The above is the lowest level abstraction in Spring 5.

### Support for alternative Reactive Streams APIs
They can adapt/bridge other Reactive Streams libraries like RxJava, even if they are using Reactor internally.

### Example
```
public class MyHttpHandler implements HttpHandler {
    Mono<Void> handle(ServerHttpRequest req, ServerHttpResponse res) {
        return res.writeWith(req.getBody());
    }
}
```

In the above, the body is a publisher and the response expects a publisher to writeWith.

### Containers support
* Tomcat, Jetty
* Servlet 3.1+ containers
* Netty
* Undertow

Netty is very heavily used for async and non-blocking code.

For Servlet 3.1+ containers, Spring bridges the Servlet 3.1 non-blocking IO and reactive streams (with back-pressure!).

### Reactive Object Mapping
They provide APIs to convert between Flux<DataBuffer> to/from Flux<T>, for example for JSON (using Jackson) and XML (JAXB/Aalto).

They support mapping for Server-Sent Events, Zero-copy transfer of Resource instances.

### Reactive Web Controller / Reactor
Spring Web Reactive API:

```
@GetMapping("/users/{id}")
public Mono<User> getUser(@PathVariable Long id) {
    return this.userRepository.findById(id);
}

@GetMapping("/users")
public Flux<User> getUsers() {
    return this.userRepository.findAll();
}
...
```

The above is NOT Spring MVC: different contract because Spring MVC's core contract is based again on the Servlet API.

Again, the Flux<User> may have latency associated, hence the data is processed as a stream. With Spring Web Reactive, the assumption is that the code is asynchronous, it is non-blocking.

Spring Web Reactive also supports RxJava:
```
@GetMapping("/users/{id}")
public Single<User> getUser(@PathVariable Long id) {
    return this.userRepository.findById(id);
}

@GetMapping("/users")
public Observable<User> getUsers() {
    return this.userRepository.findAll();
}

@PostMapping("/users")
public Completable addUser(@RequestBody User user) {
    return this.userRepository.save(user);
}
```

### Non-blocking HTTP GET
You can return simple types if the code is non blocking:
```
...
public User getUser(@PathVariable Long id) {
    return new User(id);
}
```

### Non-blocking HTTP POST
```
...
public Mono<Void> addUser(@RequestBody User user) {
    return this.userRepository.save(user);
}
```

In the above, the User object has to be completely constructed (deserialized) before the method can be invoked.
With large payloads this can take a long time, meaning a risk of blocking code.

A non-blocking alternative:

```
public Mon<Void> addUser(@RequestBody Mono<User> user) {
    return mono.then(this.userRepository::save);
}
```

### WebClient

```
ClientHttpConnector connector = new ReactorClientHttpConnector();
WebClient client = WebClient.create(connector);

ClientRequest<Void> request = ClientRequest.GET(url)
    .accept(MediaType.APPLICATION_JSON)
    .build();

Flux<Something> result = client
    .exchange(request)
    .flatMap(response => response.body(toFlux(Something.class)));
```

Integral part of the non-blocking story. Useful for distributed apps / microservices making calls to other services.

The above API allows for writing non-blocking client code.

### Routing
They are working on a reactive router.

Provides functional interfaces for defining routes and handler functions in a functional style.

Advantage: easy to roll a main class and have a powerful application up and running.