# Node stats 

## Node statistics

The cluster nodes stats API allows to retrieve one or more (or all) of the cluster nodes statistics.
for example here:
```
curl -XGET 'http://localhost:9200/_nodes/stats'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/stats'
```
the first command retrieves stats of all nodes in the cluster, however the second command selectively retrieves stats of nodeId1 and nodeId2.

you can control the result returned by specifying the metric item.

optional metric items:
* `indices`
   * Indices stats about size, document count, indexing and deletion times, search times, field cache size, merges and flushes
* `fs`
   * File system information, data path, free disk space, read/write stats
* `http`
   * HTTP connection information
* `jvm`
   * JVM stats, memory pool information, garbage collection, buffer pools, number of loaded/unloaded classes
* `os`
   * Operating system stats, load average, mem, swap
* `process`
   * Process statistics, memory consumption, cpu usage, open file descriptors
* `thread_pool`
   * Statistics about each thread pool, including current size, queue and rejected tasks
* `transport`
   * Transport statistics about sent and received bytes in cluster communication
* `breaker`
   * Statistics about the field data circuit breaker
* `discovery`
   * Statistics about the discovery
* `ingest`
   * Statistics about ingest preprocessing
for example here
```
# return just indices
curl -XGET 'http://localhost:9200/_nodes/stats/indices'
# return just os and process
curl -XGET 'http://localhost:9200/_nodes/stats/os,process'
# return just process for node with IP address 10.0.0.1
curl -XGET 'http://localhost:9200/_nodes/10.0.0.1/stats/process'
```




