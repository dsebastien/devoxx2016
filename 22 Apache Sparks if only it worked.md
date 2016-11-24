# Apache Sparks if only it worked

## Author
@mszymani

## Spark
* engine for distributed data processing
* Java, Scala, R, Python API
* SQL support, Streaming, Machine learning

## Execution model
RDD: Resilient Distributed Data set
* API which lets you feel that you work with local collections of data, while the data distributed into partitions and spread across the cluster
  
When processing data, some of it must be exchanged between partitions. Those pipelined operations are called stages.
When we exchange the data, for example when we want to do a "group by key", you actually want to pull all the same keys to one place, that is done by an operation called "Shuffle".

Shuffle is costly: I/O & network cost associated.

Applications consist of multiple stages and multiple shuffles.

## Execution unit
Task:
* instructions of what to do (i.e., our code)
* data to process (e.g., blocks in HDFS)

Stages consist of multiple independent tasks

## Executor
Each task gets run in a JVM called an "Executor".
You control how large the executor will be, thus how much CPU/memory it can use.

Example: `--executor-cores 3 --executor-memory 10g`

When an executor is done processing a task, it picks up another one. Thus an executor is a long living JVM.

Executors are controller by a single "driver" process.

## Memory model
Heap memory split in 3 areas: spark.executor.memory
* cache: spark.storage.memoryFraction
* shuffle buffer: spark.shuffle.memoryFraction
* user program

Plus an overhead part
* memory overhead: spark.yarn.executor.memoryOverhead

Separation of each memory eare controlled by Spark itself but can be overriden (not recommended though)

## Executor sizing
Depends on the workload.

Small ones: 1CPU or vCPU
* not ideal
* Spark can benefit from executing multiple tasks in the same JVM
  * some parameters are broadcasted to all executors, meaning that many small executors will waste memory
* usually safe though
* start with small ones

Very large ones:
* often problematic (GC issues)

Advice
* keep memory overhead in mind
* keep some resources for the OS
* play with Spark, cache an RDD and the Spark UI will tell you how much memory the RDD is taking
* if a single task sometimes take a very large input and sometimes very small ones, you can use "dynamic resource allocation"
  * starts with a small number of executors and increases as required (also kills executors that are not needed anymore)

## Shuffle
Once a task has finished executing, the processed data is split into buckets and these get shuffled (moved to another stage), depending on what needs to be done next.

Spark has a limitation of 2GB per data block when doing shuffle.

What to keep an eye on:
* failing tasks
* GC heavy tasks
* shuffle read/write sizes
* long running tasks

Questions to ask:
* too much data per task?
* is parallelism level large enough?
  * def groupByKey(numPartitions: Int)
  * def repartition(numPartitions: Int)
* enough memory for the executor?

## Execution plan
Spark UI can be used to visualize the execution plan. That shows the stages through which the data goes (i.e., the data processing pipeline)

## Skewed data
Sometimes increasing the partitioning is not enough. Sometimes data is skewed. For example some keys are much more common than other ones.
You can end up having a lot of tasks, and many of these basically doing nothing while a few have to handle a lot of data.

The solution is to add some "salt" to the keys to more evenly distribute the workload.

## Locality
You want to run tasks that process data on the same machines where the data currently is.
Concept borrowed from Hadoop.

Go to Spark UI -> Locality Level can be visualized

To improve it:
* increase the number of executors
* spark.locality.wait parameter
  * hint to spark for how long it should wait to be able to run a specific task with a certain locality level
  * default: 3 seconds

## Caching
RDDs get lazily evaluated. If Spark is instructed to read some data then to apply a map, filter or flatMap, nothing actually happens on the cluster.
Things only happen when an action occurs (e.g., write results to disk).

Some operations may be executed multiple times.

Spark lets you cache your RDD in memory.

* branch in execution plan is a candidate for caching
* Spark UI shows how much memory an RDD is taking
* you cannot control the priority (LRU)
* don't cache to disk if computation is cheap
* caching with RF - only when re-creation is extremely costly
* checkpoint vs caching
* shuffle data is automatically persisted

## Broadcast variables
* variable copied to each and every executor

## Optimize shuffle - recap
* control number of partitions
* use mapValues instead of map if you can
* broadcast variables
* filter before shuffle
* avoid groupByKey, use reduceByKey

## Debugging tools
* spark UI
* HDFS monitoring
* aggregate your logs
* extra java options - observe your GC
* run and test locally