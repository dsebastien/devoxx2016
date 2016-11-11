# Debugging Distributed Systems

## Zookeeper
Distributed system useful for building other distributed systems

Small in-memory file system

Key-value store

## API
* create directory
* create a file
* watch a file for changes
* atomically update a file
* create ephemeral files
* create sequential files (concurrent attempts to create are ordered)

Can be used for leader election, ...

Helps also to handle distributed locking.

## Transactions
No ACID transactions with ZooKeeper

## Zookeeper
* one zookeeper service is composed of n servers
* each node has a full copy of the data (replicas)
* as a client you can interact with any node
* writes will be sent to the leader

## Failures
When there are network issues (e.g., latency, packet loss), some followers can fall behind (i.e., didn't have all the items in the database).
Further the whole cluster can fall.

The leader can also fall behind.

Log hint: "too busy to snap, skipping"

## Fault injection
Try using sshfs: mount a remote folder using it and point ZooKeeper to it: that way, by playing with the network you can simulate disk slowness, disk failures, etc.

HL application checks are good because they can catch many different issues, but they don't tell the cause.

Monitoring ZooKeeper: "ruok" command (are you ok)

## Deep health checks
* write to one ZooKeeper key
* read from ZooKeeper key

## Threads (leader)
Chain of threads that process requests coming into Zookeeper
One thread per follower

## ZooKeeper heartbeat
Heartbeat from the leaders to the followers. If followers don't receive a heartbeat after a certain amount of time, they start an election and select a new leader.

The cluster is supposed to recover if the leader is killed: another leader is takes over.

Force the followers to fall behind the leader: `tc qdisc add dev eth09 root netem delay 500ms 100ms loss 35%`

## IPsec
Used to encrypt everything between leader and followers.

## Lessons
* don't lock and block
* TCP can block for a really long time
* interfaces / abstract methods make analysis harder
* automate debug info collection (stack traces, heap dumps, transaction logs, etc)
* application/dependency checks should be deep health checks
* leader/follower heartbeats should be deep checks