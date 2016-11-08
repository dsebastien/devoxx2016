# Reactive Programming

## Links
* http://www.reactivemanifesto.org
* http://www.agiledeveloper.com/codesamples

## What and why
* Rehashed programming model that emphasizes on scalability and response
* A way to build a scalable architecture that's resilient and quick to react to stimuli
* All about creating an architecture that supports
  * Elastic
  * Message-Driven
  * Responsive
  * Providing responsiveness
    * Efficiency is attained not by doing tasks faster, but by avoiding those that shouldn't be done in the first place

Cases where CRUD doesn't make sense: focus on processing data, wherever it comes from and wherever it goes afterwards.
Look at processing as elements in a water pipe.

Radio analogy: continues streaming data whether we listen or not.

Every single technical decision should have an expiration label on it.

What's a reactive application? It readily responds to events, failures, load users

Why not status quo?
* multi-threading
  * Runnable suck
  * Callable suck
  * both force us to use a poor programming model (can't give it data directly, can't get data back directly)
* future: low level of abstraction, hard to compose, easy to block on gets - poor utilization
* callback: don't compose well, they're horrible

Solution? Dataflow and composition of events

Difficulty with exception handling when doing multi-threading

Dataflow computing is what reactive programming is all about: handle streams of data

## From functional to reactive
Essential about functional programming:

* Immutability
* Higher order functions
  * passing functions to functions
* lazyness
* function composition

```
System.out.println(
  symbols.stream()
    .map(String::toLowerCase)
    .collect(toList())
);
```

Reactive programming starts where functional programming leaves us.

## Observable
What it is?
* nothing but a data flow, a data stream
* like a radio you can turn on and you have the program that streams to it
* difference from an iterator?
  * with iterators we can pull the data out of a collection, one item at a time
  * with observables, they push the data when the data is available/ready
* may be synchronous or asynchronous

How is it different from the observer pattern?
* it IS the observer pattern, plus...
  * it can signal the end of a data stream
  * propagate errors (resilience!)
    * in functional programming, we're going downstream, so errors just flow downstream too
      * contrary to imperative programming where we step back when errors occur
  * evaluation may be synchronous, asynchronous, or lazy

## Streams vs Observables?
* stream is a single pipeline: not possible to have multiple subscribers
* observable has clear channels for data, errors, completion
* observable is a higher level abstraction -> Observable = stream++

It's important to unsubscribe when we're not interested in the next values for a given stream.

In Java 9, takeWhile will allow to stop processing streams (e.g., example where a subscriber unsubscribes and the observable does not want to emit items to it anymore from that point on)

## Making the stream asynchronous
How many threads do you want?
If your task is computation intensive, then give no more threads than the number of cores
Otherwise
* number of cores / (1 - blocking factor)
* number of cores / (1 - 0.5) = 2. number of cores

blocking factor is between 0 and < 1

Configurable with RxJava: `observable.subscribeOn(Schedulers. computation()|newThread()|...`

## Error handling
Subscribe by declaring at least the onNext and error functions:
```
something.subscribe(System.out::println, Sample::handleError);
```

After an error occurred, the stream ends directly (after pushing the error to the onError function).

We can resume by doing this:
```
oservable<StockInfo> feed = StockServer.getFeed(symbols);
feed.onErrorResumeNext(throwable -> callABackup(throwable, symbols))
  .subscribe(System.out::println, Sample::handleError);


private static Observable<StockInfo> callABackup(Throwable throwable, List<String> symbols) {
  return StockServer.getFeed(symbols);
}
```

The onErrorResumeNext is lazily evaluated.

## Transforming Observables
Observable<StockInfo> feed = StockServer.getFeed(symbols);
feed.map(stockInfo -> new StockInfo(stockInfo.ticket, stockInfo.price * 0.9))
  .subscribe(...)

Operators are actually observables themselves
* map
* filter
* take
* skip
* takeWhile: when the condition is not met anymore, it sends an unsubscribe upstream and oncomplete downstream
* skipWhile: sends no data at all until the condition is met
* zip: combine observables

## Hot vs Cold observables
With cold observables, each subscriber starts fresh, they get data that was emitted from the beginning

```
Observable<Integer> feed = StockServer.getData()
  .subscribeOn(Schedulers.io())
  .share();
```

The share operator allows to create hot observables.

## Backpressure
Sometimes there's a small part of the data that takes a lot more work to process.

```
  .onBackPressureDrop();
```

The above will just drop whatever was given in between (i.e., when you were still busy processing another item)

## Designing Reactive Systems
* partition based on user location/query
* sharding and replication (split data across machines)
* failure as first class citizen
* not an all or nothing proposition
  * network/database failures
  * provide redundancies across geographical locations
  * load related failures
* handling backpressure
* using circuit breakers
* improving performance using parallelization
* acknowledge the CAP theorem