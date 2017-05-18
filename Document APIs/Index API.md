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
     
