# Ten ways to make code suck less

## Schedule time to lower technical debt
Coined by Ward Cunningham

A freeway filled with cars is a parking lot :) (Linda Rising)

Slack time is very important

Recommended book: Guns, Germs, and Steel (Jared Diamond)

## Favor high cohesion
Reduces frequency of change

High cohesion == low cyclomatic complexity

## Favor loose coupling
Loose vs tight coupling
Eliminate coupling wherever possible

## Kent Beck's rules for simple design
* passes the tests
* revelas intention
* no duplication
* fewest elements

The rules are in priority order.

## Avoid primitive obsession
Imperative code is packed with incidental complexity

A good code should read like a story, not like a puzzle

Functional style == declarative style + higher order functions

* Imperative: have to repeat the same thing over and over again
* Declarative style: tell what to do, but not explain how to do it

## Prefer clear code over clever code
Programs must be written for people to read, and only incidentally for machines to execute (Abelson & Sussman)

There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies and the other is to make it so complicated that there are no obvious deficiencies (Tony Hoare)

## Apply Zinsser's principle on writing
Book: On writing well (William Zinsser): book about writing non-fiction

* simplicity
* clarity
* brevity
* humanity

## Comment why, not what
Don't comment to cover up bad code

Write expressive code that is self-documenting.

Good code is like a good joke: people have to get it

Those who can't design are condemned to document

## Avoid long methods
Turns out long is not about length of code, but levels of abstraction.

Apply SLAP

## Give good meaningful names
We must make all we can to optimize code for reading, not writing!

Programmer: one who can name their children quite easily but has a hard time naming their variables

If we can't name a variable or a function appropriately, it may be a sign we've not yet understood its true purpose.

## Do tactical code reviews
Review tests first, then the code behind, as intention should be clear.

Perspective-based review catch more defects than nondirected reviews. Peer reviews complement testing
Code reviews are technical & social

## Reduce state and state mutation
Avoid starting out with variables, private fields etc. Concentrate on methods & functions, behavior, inputs & outputs. Let variables/fields come along when actually needed

Think more, type less. Aim for minimalism, fewer states, less mutability and just enough code for the known, relevant parts of the problem.

Messing with state is the root of many problems.

Functional style helps reduce mutability.

## Conclusion
Let go of the conviction that you can write code once and get it right the first time around ;-)