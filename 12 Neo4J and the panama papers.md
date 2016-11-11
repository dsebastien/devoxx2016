# Neo4j and the Panama papers

## Links
* https://neo4j.com/

## Neo4j

Graph database
* manage and store your connected data as a graph
* query relationships easily & quickly
* evolve model and applications to support new requirements and insights
* built to solve relational pains
* built for connected data
* easy to use
* optional schema for the data
* highly scalable
* transactional (ACID) database

Cassandra is not a "graph-native" solution. (less performant for working with graphs)

Nodes
* entities in the graph
* can have name-value properties
* can be labeled

Relationships
* relate nodes by type and direction
  * the direction does not imply the switch direction

## Neo4j common use cases
* internal applications
  * master data management (MDM)
  * network and it operations
  * fraud detection

* customer-facing applications
  * real-time recommendations
  * graph-based search
  * identity and access management

## Patterns
Can be inferred from the graph model

MATCH(:Person { name: "Dan }) -[:KNOWS]->(who:Person) RETURN who
        label     property   ...

## Cypher: clauses
* CREATE pattern
* MERGE pattern: create something if it doesn't exist yet
* SET: change a node (set new labels, change, etc)
* DELETE
* REMOVE: remove properties
* WHERE <predicate>: filter
* MATCH: pattern
* SKIP
* RETURN
* WITH <expression> as <alias>: like the pipe in unix shells 
* UNWIND <list> as <item>
* LOAD CSV from <..>: easiest to get data in neo4j
  * transactional (ACID) writes
  * initial and incremental loads of up to 10M nodes and relationships
* CSV bulk loader
  * for initial database population
  * for loads with 10B+ records
  * format important

