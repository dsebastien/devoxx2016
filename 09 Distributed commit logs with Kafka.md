# Distributed commit logs with Kafka

## Links
* https://kafka.apache.org/
* Reactive Kafka client (Akka): https://github.com/akka/reactive-kafka
* Reactive Kafka client (RxJava): https://github.com/cjdev/kafka-rx
* Reactive Kafka client (Reactor): https://github.com/reactor/reactor-kafka

## (bad) analogy
Analogy with RAID5 and its impact.

Kafka = Event stream, Distributed & redundant

## Kafka fundamentals
* messaging system semantics: pub/sub (e.g., jms)
  * Kafka uses the same concepts/terminology
* clustering is core
* durability & ordering guarantees

## Use cases
* modern ETL (extract transform load) / CDC (change data capture)
  * Kafka is becoming a replacement for those systems
* data pipelines
* big data ingest: providing and endpoint where all systems can come and publish data, subscribe, etc
  * scalable buffer

## Records
Kafka uses records (message, event, etc): container sending information through the system
Record:
* name, value, timestamp
* immutable
* append only (no update)
* persisted across the cluster and to disk
  * can configure lifetime, etc

Records in Kafka are logs

## Producers and Consumers
In Kafka, there is a broker (node in the cluster)
* Producers write messages to a broker
* Consumers read records from a broker

Kafka doesn't do push, clients ask if there are records (i.e., pull-based approach)

Kafka uses a leadeR/follower for cluster distribution: there will be an elected leader for a given part of Kafka and there will be followers that are getting the records being pushed to the leaders

## Topics & partitions
Can send messages to a topic (producers) and get messages from a topic (consumers).

* topic: logical name with 1 or more partitions.
* tartition: parts of a topic; allow to parallelize the processing for a given topic on multiple nodes
  * are replicated in the cluster (managed by Kafka itself)
  * ordering is guaranteed for a partition

## Offsets
As messages are added to a partition they get assigned a unique sequential id.
Consumers track offsets

The ordering is only in a specific partition (each partition is ordered/indexed separately)

Benefits: replay, different speed consumers, etc

## Consumer groups
Create a logical name of 1-n consumers

## Clients
Many clients available.
* JVM has official support
* polling based

## Akka streams
* Akka is an actor system
* implementation of Reactive Streams
* uses a Source / Sink
* support back-pressure

https://github.com/akka/reactive-kafka

## Zookeeper
Kafka uses Zookeeper to store information about the cluster.

