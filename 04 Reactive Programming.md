# Reactive Programming

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

## Example 1
