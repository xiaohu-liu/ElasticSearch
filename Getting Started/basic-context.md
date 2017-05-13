### Basic Concepts
* 7 basic concepts that are core to ElasticSearch
* basic concepts listed as below
    * Near RealTime
    * Cluster
    * Node
    * Index
    * Type
    * Document
    * Shard 
    * Replica
    
###  Near RealTime
* Eleatisearch is a near real time search platform
* a slight latency (`normally one second`) between the time you index a document and the time it is get available

### Cluster
What is a cluster of elasticsearch?
* a collection of one or more nodes that together holds the `entire data` and provides `federated indexing` and `search capabilities` across all nodes

Cluster Name
* a cluster is identified and organized by a unique name which by default is `elasticsearch`
  * a node can only be part of a cluster if the node is set up to join the cluster by its name

<strong>NOTE</strong>: 
* it is valid and fine that a cluster just have one node
* you can have multiple cluster each with it's unique name

### Node
* one server of your cluster
* stores data, participates in cluster indexing and search capabilities
* one unique name per node in the cluster( default `uuid`)
* you can define for each node to identify which servers is responsed to which nodes in the cluster
* one node can join the cluster by specifing the cluster name (`elasticsearch` by default)  when to start


### Index
* a collection of documents that have somewhat similar characteristics
* identified by index name given
* use the name to refer to the index when performing indexing, search, update, and delete operations against the documents in it
* you can define as many indexes as you want


### Type
* diffent `categories` or `partitions` for one index
* be defined for documents that have a set of common fields

### Document
* unit information that can be indexed
* with one index/type , you can store as many documents as you want
* one document stored phsically in one index , it actually must be indexed and assigned to the type within the index

### Shard
* a index can be subdivied into mutiple pieces called shards
* you can define how many shard one index holds when it is created
* per shard is just a full-functional and an independent index that can be hold on any node in the cluster

Shard is important for 2 primary reasons:
* split/scale your content volume horizontally
* distribute and parallelize operations accross shards thus incresing the performance and throughput


### Replica
failover mechanism in case a shard/node somehow goes offline or disappears for whatever reason
  * make more than one replica for per shard of one index
Replica is important for 2 primary reasons?
* never allocate on the same server/node as the primary shard that is copied from
* scale out/in your search volume/throughput for that search can be executed on all replication in parallel



### Summary
* one index consists of multiple shards
* one index can be replicated 0 or more times
* if replicated , an index consists of the primary shards and replica shards
* you can defined with custom the number of shards and replica for per index at the time  the index is created
* you can dynamically change the number of replicas but you can not change the number of shards after-the-fact
* by default, Index is allocated 5 primary shards and 1 replica

<strong>Note:</strong> Each elasticSearch shard is a lucene index . and a single lucene index can have Integer.MAX_VALUE documents, you can monitor the shard size using the _cat/shards api.






