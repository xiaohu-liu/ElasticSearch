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


### List All Indics
we can get the indics of the cluster as follow:
```
GET /_cat/indices?v
```
Response as follow:
```
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
```
Which simply means we have no indices yet in the cluster.



### Create Index
let's try to create an index named `customer` and then list the indexes hold in the cluster as follow
```
PUT /customer?pretty
GET /_cat/indices?v
```
<strong>Note</strong>: use the `PUT` verbs to send the create request and the `pretty` to pertty-print the json response

Response as follow:
```
health status index    uuid                   pri rep docs.count docs.deleted store.size  pri.store.size
yellow open   customer 95SQ4TSUT7mWBT7VNHH67A   5   1          0            0       260b           260
```
as we can see from the response text above, the index named `customer` has 5 primary shards and 1 replica per shard, and there is no 
any document in the index.

you may notice that the index named `customer` has a yello health status tagged to it, as we all know , the yellow health status means that there are some replicas may not be allocated, the reason for that is there is just only one node in the cluster , the one replica can not be allocated until a new node join the cluster , once another node joins the cluster , the one replica will be allocated  onto the new one , the health status will turn to  green


### Index and Query a document
In order to index a document you must tell the elasticsearch which type in the index it should go to 

let us index a customer document into `customer` index and `external` type , with one id as 1 as follow:
```
PUT /customer/external/1?pretty
{
  "name": "John Doe"
}
```
Response as follow:
```
{
  "_index" : "customer",
  "_type" : "external",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "created" : true
}
```
we can see from the response text above , the document was be succefully indexed into `customer` index and `external` type , and it's id
is just the value `1` specified at index time.

<strong>Note:<strong> It's not neccessary to implicitly create the index we requested , the elasticsearch will automatically create the index if it doest not exist beforehead.

### Query the document 
let's retrive the document that we have indexed before as follow
```
GET /customer/external/1?pretty
```
Resonse as follow
```
{
  "_index" : "customer",
  "_type" : "external",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "_source" : { "name": "John Doe" }
}
```
fields notes:
```
_index: the index of the document belongs to
_type: the type of the document index into
_id: the precise id if we specified or uuid is not specified
_source: the full document content we indexed
```



