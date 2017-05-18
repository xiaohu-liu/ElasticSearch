# Reading and Writing documents

## data replication model
as we all know , each index in es will be divided into shards and each shard can have multiple copies.
the copies for one shard must be kept in sync when docments are added or removed
if we fail to do so , reading from one copy will result in very different results than reading from another.

## primary-backup model
es's data replication model is based on the primary-backup model. that model is based on having a single copy from the replication group
that acts as the primary shard. the other copies are called replica shards.

The primary shard serves as the main entry point for all indexing operations. it is in charge of validaing them and making sure they are correct . 


Once an index opreation has been accepted by the primary , the primary is also responsible for replicating the operation to the other copies.


## Basic write  model
every indexing operation in es is first resolved to a replication group using `routing`. once the replication group has been determinated, the operation is forwarded internally to the current primary shard of the group. 


the primary shard is responsible for validating the operation and forwarding it to the other replica shards. since replicas can be offline, there is not required for primary to relicate to all replicas. instead, es maintains a list of shard copies that should received 
the operation. <strong>This list is called in-sync copies and is maintained by the master node.</strong> As the mane implies, there are the set of "good" shard copies that are guaranteed to have proccessed all the index and remove operations that have bean acknowleged to the user, the primary is responsible for maintaing this invariant and thus to replicate all operation to each copy in the set.


what the primary shard should do:
* validate incoming operation and reject it if structurally invalid
* execute the operation locally, i.e. indexing or deleting the relevant document. this will also validate the content of fields and reject it if needed
* forward the operation to each replica in the current in-sync copies set. if there are mulitple replcias, this is done in parallel.
* once all relicas have successfully performed the operation and responsed to the primary. the primary acknowlegess the successful completion of the request to the client.


## Failure Handling
Many things can go wrong during indexing
* disk can get corrupted
* nodes can be disconnected
* some configuration mistake
the reasons above listed could cause an operation fail on a replica despite it being successful on the primary. the primary has to response to them.

in the case that the primary itself fails, the node hosting the primary will send a message to the master about it. The indexing operation will be hold for 1 min by default for the master to promote one of the replicas to be as the new primary. the operation will be forwarded to the new primary for processing

<strong>Note that: the master also monitors the health of the nodes and my decide to promactively demote a primary. This typically happens when the node holding the primary is isolated from the cluster</strong>

Once the operation has been successfully performed on the primary. the primary has to deal with potential failures when executing it on
the replica shards. This may be caused by an actual failure on the replica or due to a network issue preventing the operation from reaching the replica. All of these share the same end result: a replica which is part of the in-sync replica set misses an operation that is about to be acknowledged. In order to avoid violating the invariant, the primary sends a message to the master requesting that the problematic shard be removed from the in-sync replica set. Only once removal of the shard has been acknowledged by the master does the primary acknowledge the operation. Note that the master will also instruct another node to start building a new shard copy in order to restore the system to a healthy state.

While forwarding an operation to the replicas, the primary will use the replicas to validate that it is still the active primary. If the primary has been isolated due to a network partition (or a long GC) it may continue to process incoming indexing operations before realising that it has been demoted. Operations that come from a stale primary will be rejected by the replicas. When the primary receives a response from the replica rejecting its request because it is no longer the primary then it will reach out to the master and will learn that it has been replaced. The operation is then routed to the new primary.


## Basic Read Model
When the read request is received by a node, the node is responsible for forwarding it to the nodes that hold the relevant shards, collatin the responses , and responding to the client. We call the node the `coordinating node` for the request. the basic flow is as follows:
* Resolve the read requests to the relevant shards.
* Select an active copy of each relevant shard, from the shard replication group. This can be either the primary or a replica. By default, Es will simply round robin between the shard copies.
* Send shard level read request to the selected copies.
* Combine the results and response. Note that in the case of get by ID look up, only one shard is relevant and this step can be skip.

## Failure handling 
When a shard fails to response to a read request, the `coordinating node` will select another copy from the same replication group and send the shard level search request to that copy instead. Repetive failurs can result in no shard copies being available. In some cases, such as _search, Es will prefer to response fast, albeit with partial results, instead of waiting for the issule to be resolved.
















