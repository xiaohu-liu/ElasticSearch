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
* different name and different cluster

<strong>NOTE</strong>: 
* it is valid and fine that a cluster just have one node
* you can have multiple cluster each with it's unique name


