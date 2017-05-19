# Delete APi
Delete APi allows you to delete the document from a specific index based on its id.
Here is an example as follows to show the api:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X DELETE 'http://localhost:9200/twitter/tweet/AVweZoDBB3_RrgjQvGlT?pretty=true'
{
  "found" : true,
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVweZoDBB3_RrgjQvGlT",
  "_version" : 2,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```

## Versioning
Each document indexed is versioned. When deleting a document, the `version` can be specified to make sure the revelant document we are trying to delete is actually being deleted and it has not changed in the meantime. Every write operation included delete on a document causes its version to be incremented.

## Routing 
when using the ability to control the routing to index a document, in order to delete the document, the routing  value should also be provided , for example: 
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X DELETE 'http://localhost:9200/twitter/tweet/AVwedqYHB3_RrgjQvGlX?pretty=true&routing=xiaohu-liu1'
{
  "found" : true,
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwedqYHB3_RrgjQvGlX",
  "_version" : 2,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```
<strong>Note: </strong> 
* issuing a delete without the correct routing value will cause the document to not be deleted.
* when the `_routing` mapping is set as `required` and no routing value is specified, the deleted api will throw a `RoutingMissingException` and reject the request.

## Parent
the `parent` parameter can be set , which will basically be the same as the routing parameter.
<strong>Note: </strong>
* delete a parent document does not automatically delete its children.
* one way to delete all child documents given a parent's id is to use the `Delete By Query API` to perform a index with the automatically generated  field `_parent` , which is in the format parent_type#parent_id
* when deleting a child document its parent id must be specified , otherwise the delete request will be rejected and a `RoutingMissingException` will be thrown instead.

## Automatic Index creation
the delete operation automatically creates an index if the index has not been created before, and also automatically creates a dynamic type mapping for the specific type if it is has not been created before.


## Distributed
The delete operation gets hashed into a specific shard id , it then gets redirected into the primary shard within that id group. and replicated to shard replicas within that id group.

## Wait for Active shards
when making delete requests, you can set the `wait_for_active_shards` parameter to require a minimum number of shard copies to be active
 before starting to process the delete request.
 
 
## Refresh
Control when the changes made by this request are visible to search.

## Timeout
The primary shard assigned to perform the delete operation might not be available when the delete operation is executed, Some reasons for this might be that the primary shard is currently recovering form a store or undergoing relocation. By default, the delete operation
will wait on the primary shard to become available for up to 1 min before failing and responding with an error. The `timeout` parameter can be used to explicitly specify how long it waits. Here is an example of setting it to 5 min:
```
$ curl -XDELETE 'http://localhost:9200/twitter/tweet/1?timeout=5m'
```






