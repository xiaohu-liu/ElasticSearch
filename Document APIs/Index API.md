# Index API
adds or updates document to a specified index, making it searchable
let's take an example as follows:
```
PUT twitter/tweet/1
curl -XPUT http://localhost:9200/twitter/tweet/1?pretty --data '{"user" : "kimchy","post_date" : "2009-11-15T14:12:12", "message" : "trying out Elasticsearch"}'
{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}
```
and the response text as follows:
```
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "2",
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

the call above indexes the document into `twitter` index and `tweet` type.
let's take look at some fields in response text:
```
total - Indicates to how many shard copies (primary and replica shards) the index operation should be executed on.
successful- Indicates the number of shard copies the index operation succeeded on.
failed - An array that contains replication related errors in the case an index operation failed on a replica shard.
```

## Automatic Index Creation
### Note
* the index operation automatically creates an index if it has not been create
* the index operation automatically creates an dynamic type mapping for the specific type if one has not yet been created
### Configuration
* action.auto_create_index(automatic index creattion)
     * disbaled it when to set  false
     * enable it when to set true
* index.mapper.dynamic(automatic mappping creation)
     * disabled it when to set false
     * enable it when to set true
* white/black list
     * + meaning allowed(e.i. +aaa*,+ccc*,+*)
     * - meaning disallowed(e.i. -aaa*, -*)
     
     
## Operation Type
The index operation also accepts an `op_type` that can be use to force a `create` operation, allowing for "put-if-absent" behavior.
when `create` is used, the index operation will fail if a document by that id already exists in the index.

Here is an example of using the `op_type` parameter:
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPUT "http://localhost:9200/twitter/tweet/2?pretty=true&op_type=create"  --data '{"user" : "xiaohu-liu","post_date" : "2009-11-15T14:12:12", "message" : "trying out Elasticsearch"}'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "version_conflict_engine_exception",
        "reason" : "[tweet][2]: version conflict, document already exists (current version [1])",
        "index_uuid" : "lHkiUFEmTrKYugulAenzgA",
        "shard" : "2",
        "index" : "twitter"
      }
    ],
    "type" : "version_conflict_engine_exception",
    "reason" : "[tweet][2]: version conflict, document already exists (current version [1])",
    "index_uuid" : "lHkiUFEmTrKYugulAenzgA",
    "shard" : "2",
    "index" : "twitter"
  },
  "status" : 409
}
```
as we can see from the response above, the create operation fails for the `version_confilict_engine_exception`

there is another option to specify the `create` is to use the following uri:
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPUT "http://localhost:9200/twitter/tweet/2/_create?pretty=true"  --data '{"user" : "xiaohu-liu","post_date" : "2009-11-15T14:12:12", "message" : "trying out Elasticsearch"}'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "version_conflict_engine_exception",
        "reason" : "[tweet][2]: version conflict, document already exists (current version [1])",
        "index_uuid" : "lHkiUFEmTrKYugulAenzgA",
        "shard" : "2",
        "index" : "twitter"
      }
    ],
    "type" : "version_conflict_engine_exception",
    "reason" : "[tweet][2]: version conflict, document already exists (current version [1])",
    "index_uuid" : "lHkiUFEmTrKYugulAenzgA",
    "shard" : "2",
    "index" : "twitter"
  },
  "status" : 409
}
```
the same error occurs as the precious call.

## Atomatic ID Generation
the index operation can be exectuted withoud specifying the id , in such a case , an id will be generated automatically, in addition, the `op_type` will automatically be set to `create`, let's take the example below to indicate that:
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPOST "http://localhost:9200/twitter/tweet?pretty=true"  --data '{"user" : "xiaohu-liu","post_date" : "2009-11-15T14:12:12", "message" : "trying out Elasticsearch"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwaomY7B3_RrgjQvGlK",
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

## Routing
by default, shard placement is controlled by using a hash of the document's id value. for more implicit control, the vlaue fed into the hash function used by the router can be directly specified on a pre-operation basis using the `routing` parameter, for example:
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPOST "http://localhost:9200/twitter/tweet?pretty=true&routing=xiaohu-liu"  --data '{"user" : "xiaohu-liu","post_date" : "2009-11-15T14:12:12", "message" : "trying out Elasticsearch"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwapZvHB3_RrgjQvGlL",
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
 in the example above, the document is routed to a shard based on the `routing` parameter provided:`xiaohu-liu`
 * `_routing` value can be extracted from the document itself when setting up explicit mapping and the `_routing` field can be optionally used to direct the index operation
 * if the `_routing` mapping is defined and se to `required`, the index operation will fail if no routing parameter value is provided or extracted
 
## Parent && Child
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPUT "http://localhost:9200/blogs?pretty" --data '{"mappings": { "tag_parent": {},"blog_tag": {  "_parent": { "type": "tag_parent" }   }  }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}
```
```
[xiaohu-liu@cdh1 elasticsearch-5.4.0]$ curl -XPUT "http://localhost:9200/blogs/blog_tag/1122?parent=1111&pretty=true" --data '{"tag":"something"}'
{
  "_index" : "blogs",
  "_type" : "blog_tag",
  "_id" : "1122",
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
when indexing a child document, the routing value is automatically set to be the same as its parent , unless the routing value is explicitly specified using the `routing` parameter.


## Distributed
The index operation is directed to the primary shard based on its route, and performed on the actual node containing this shard, After the primary shard completes the operation, if needed, the update is distributed to applicable replicas.

## Wait For Active Shards
To improve the resiliency of writes to the system. indexing operations can be configure to wait for a certain number of active shard copies before proceeding with the operation. if the requisite number of active shard copies are not available, then the write operation will wait and retry until either the requisite shard copies have started or a timeout occurs.

* By default, write operations only wait for the primary shards to be active before processing(i.e. wait_for_active_shards = 1).
* the setting can be dynamically overriden by setting `index.write.wait_for_active_shards`
* to alter this hebavior per operation, the `wait_for_active_shards` request parameter can be used.
* all or any positive integer up to the total number of configured copies per shard in the index (number_of_replica + 1)
* a negative or a number greater than the number of shard copies will throw an error

It is important to note that this setting greatly reduces the changes of the write operation not writing to the requisite number of shard copies, but it dose not compeletly eliminate the possibility, since the check occurs before the write operation commences.



     
