### The REST API
how can we communicate with our cluster?
the elasticsearch provides a comprehensive and powerfully REST API that we can use to communicat with our cluster.

What can we do with the REST API?
* Check your cluster, node, and index health, status, and statistics
* Administer your cluster, node, and index data and metadata
* Perform CRUD (Create, Read, Update, and Delete) and search operations against your indexes
* Execute advanced search operations such as paging, sorting, filtering, scripting, aggregations, and many others



### HealthCheck
we can use http client to communicate with our cluster , such as `curl` commond line, `postman`, `java http api` client ,etc.
To check the cluster health , we can use the url as follow:
```
GET /_cat/health?v
```
response as follow:
```
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1475247709 17:01:49  elasticsearch green           1         1      0   0    0    0        0             0                  -                100.0%
```

what the status can give us?
* Green means everything is good (cluster is fully functional)
* Yellow means all data is available but some replicas are not yet allocated (cluster is fully functional)
* Red means some data is not available for whatever reason. 
<strong>Note</strong> that even if a cluster is red, it still is partially functional (i.e. it will continue to serve search requests from the available shards) but you will likely need to fix it ASAP since you have missing data.

Elasticsearch uses unicast network discovery by default to find other nodes on the same machine, it is possible that you could accidentally start up more than one node on your computer and have them all join a single cluster.

### How to get the list of node in the cluster?
we can get the list of node in the cluster as follow:
```
GET /_cat/nodes?v
```
Response as follow:
```
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           10           5   5    4.46                        mdi      *      PB2SGZY
```

<strong>Note:</strong> the word "PB2SGZY" in the response text that is the name of the node in our cluster












