# Thinking in parallel

## Approaches and issues
* iterative approach (for loops)
* streams (aggregate)

For-loop processing
* sequential
* left-to-right ordering
* efficient/familiar/straightforward

Accidental, not essential
* key: each computation is independent of all others
* for-each loop helps but only a little (no need to specify the boundaries)

With streams: aggregate operation, not individual
* stream version is more compact, more functional (style)

Streams version is better because
* higher level of abstraction
  * expresses independence of each computation
  * less accidental complexity (eg index computations)
  * implicitly operates on all elements, not individual elements
  * focus on desired results than mechanics of computing it
* independent computations are independent
* problem setup treats all cases uniformly
* operations on aggregates, not element-at-a-time
* no loop mechanics to worry about

Making the code parallelizable isn't what makes it better
* you might never want to run this code in parallel
* code is still better

Making the code better makes it parallelizable, as a bonus.

Useful technique: stream over indexes, not elements.

## Parallelism
* parallelism is about using more resources to get the answer faster
  * strictly an optimization
  * if additional resources are not available, can still compute sequentially
* corollary: only useful if it really does get the answer faster
* just because we use more resources
  * doesn't mean the computation is always faster than a sequential one
  * or even as fast...
* analyze -> implement -> measure -> repeat
  * prefer sequential implementation until parallel is proven effective
* measure of parallel effectiveness is speedup
  * how much faster or slower

A parallel computation always involves more work than the best sequential alternative
  * still has to solve the problem
  * decompose the problem
  * launch tasks, manage tasks, wait for tasks to complete
  * combine results

Parallel version always starts out "behind"
* hopping to make up for this initial deficit by burning more resources
* to succeed we need
  * a parallelizable problem
  * a good implementation
  * good runtime support for execution
  * enough data

## Towards parallel computation
* accumulator pattern
  * need to unlearn this
  * impediment to parallelism (e.g., sum variables)

## Divide and conquer
* standard tool for parallel execution is divide and conquer
* partition the input into chunks that can be independently operated on
* recursively decompose problem until it is small enough for sequential

Recursive decomposition
* simple
* especially with recursively defined data structures like trees
* no shared mutable state -- just partitioned reading
* intermediate results live on the stack

Starts forking work early
* beware Amdahl's law

Decomposition is dynamic
* can incorporate runtime knowledge of core count and load
* portable expression of parallel computation

## Performance considerations
* splitting/decomposition costs
* task dispatch/management
* combination costs (partial results)
* locality: elephant in the room
* each can steal away potential speedup: in general, need a lot of data to make up for decomposition startup

## Parallel stream Performance
* streams is about possibly-parallel, aggregate operations on datasets
* efficient and (usually) merge computation into a single pass
* not magic :)
* still have to ensure that our problem is amenable to parallel solution
  * how easily splittable is the source
  * how costly is the result computation
  * what kind of locality does our computation get

## NQ Model
* simple model for parallel performance
* N = data items (number)
* Q = work required per item

Rule of thumb: NQ > 10000 to have a chance for parallel speedup

Most simple stream examples have very low Q: meaning everything else has to go well to get a speedup

## Source splitting
* some sources split better than others: cost of computing split, evenness of split, predicability of split
* arrays split cheaply, evenly and with perfect knowledge of split sizes
  * linked lists have none of these properties
  * iterative generators behave like linked lists
  * stateless generates behave like arrays
* compare
  * `IntStream.iterate(0, i > i+1).limit(n).sum()`
  * `IntStream.range(0,n).sum()`

## Locality
* locality is the elephant in the room
* parallelism only wins when we can keep the CPUs busy doing useful work
  * waiting for cache misses is not useful work
* memory bandwidth is often the limiting factor on many systems
* array-based, numeric problems parallelize best

## Encounter order
* some operations have semantics tied to encounter order
  * order implied by the source
  * some sources have no defined encounter order (eg hashset)
  * operations like limit, split and findFirst are tied to encounter order
  * less exploitable parallelism
* sometimes the encounter order is meaningful, sometimes not
  * call .unordered() to indicate order is not meaningful to you
  * operations like limit(), skip() and firstFirst() will optimize in the presence of unordered sources

## Merging
* for some operations (sum, max), the merge is really cheap
* for others (groupingBy to a HashMap), it is insanely expensive
  * involves a lot of copying
  * repeatedly up the tree
  * cost of merging overwhelms the parallelism advantage

Can be A LOT slower that a non parallel implementation

## Parallel streams
* any of the following factors can undermine speedup
  * NQ is insufficiently high
  * cache-miss ratio is too high
  * source is expensive to split
  * result combination cost is too high
  * pipeline uses encounter-order sensitive operations

## Conclusions
* strams are cool
* parallelism is cool
* parallelism is an optimization
* before optimizing, always...
  * have actual performance requirements
  * have reliable performance measurements
  * ensure that your performance doesn't meet requirements
