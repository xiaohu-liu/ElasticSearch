# GET API
The get API allows to get a typed json document from the index based on its id.
Here is an example to show that:
```
[xiaohu-liu@cdh1 bin]$ curl -XGET "http://localhost:9200/twitter/tweet/1?pretty=true"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "1",
  "_version" : 7,
  "found" : true,
  "_source" : {
    "user" : "xiaohu-liu",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
  }
}
```
the above result includes the `_index`,`_type`,`_id` and `_version` of the document we wish to retrive. including the actual `_source` of the document if it could be found.

The API also allows to check for the existence of a document using `HEAD`, for example as follows:
```
[xiaohu-liu@cdh1 bin]$ curl -i -X HEAD "http://localhost:9200/twitter/tweet/1"
HTTP/1.1 200 OK
content-type: application/json; charset=UTF-8
content-length: 182
```
## RealTime
By default, the get api is realtime, and is not affected by the refresh rate of the index, if a document has been updated but is not yet  refreshed , the get api will issue a refresh call in-place to make the document visible. this will make other documents changed since the last refresh visible . in order to disbaled realtime GET, one can set the realtime paramete to `false`.

## Optional Type
the `_type` is optional in GET API, set it to `_all` will fetch the first document matching the id across all types.

## Source Filtering
by default, the get operation returns the contents of the `_source` field unless you have used the `stored_fileds` parameter or if the `_source` field is diabled, you can use the `_source` parameter to turn off the `_source` retrieval. for example:
```
[xiaohu-liu@cdh1 bin]$ curl -X GET "http://localhost:9200/twitter/tweet/1?pretty=true&_source=false"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "1",
  "_version" : 7,
  "found" : true
}
```
if you just need one or two field from the complete `_source`, you can use the `_source_include` & `_source_exclude` parameters to include or filter out the parts you need. this can be especially helpful with large documents where partial retrieval can save on network overhead. Both parameters take a comma separated list of the fields or wildcard expressions. for example:
```
[xiaohu-liu@cdh1 bin]$ curl -X GET "http://localhost:9200/twitter/tweet/1?pretty=true&_source_include=*&_source_exclude=user"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "1",
  "_version" : 7,
  "found" : true,
  "_source" : {
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
  }
}
```

if you only want to specify includes, you can use the shorter notation:
```
[xiaohu-liu@cdh1 bin]$ curl -X GET "http://localhost:9200/twitter/tweet/1?pretty=true&_source=*user,message"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "1",
  "_version" : 7,
  "found" : true,
  "_source" : {
    "message" : "trying out Elasticsearch",
    "user" : "xiaohu-liu"
  }
}
```

## Stored Fields
The GET API allows specifying a set of stored fields that will be returned by passing the `store_fields` parameter, if the requested fields are not stored, they will be ignored. for example:
```
[xiaohu-liu@cdh1 tmp]$ curl -X PUT "http://localhost:9200/xiaohu-liu?pretty=true" -d "@mapping.json"
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}
mapping.json
{
   "mappings": {
      "tweet": {
         "properties": {
            "counter": {
               "type": "integer",
               "store": false
            },
            "tags": {
               "type": "keyword",
               "store": true
            }
         }
      }
   }
}
```
Now we can add a document:
```
[xiaohu-liu@cdh1 tmp]$ curl -X PUT "http://localhost:9200/xiaohu-liu/tweet/1?pretty=true" -d '{"counter":1,"tags":["red"]}'
{
  "_index" : "xiaohu-liu",
  "_type" : "tweet",
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

so , we try to retreve it:
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET "http://localhost:9200/xiaohu-liu/tweet/1?pretty=true&stored_fields=tags,counter"
{
  "_index" : "xiaohu-liu",
  "_type" : "tweet",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "fields" : {
    "tags" : [
      "red"
    ]
  }
}
```
the `counter` field is just simply ignored.

## Generated Fields
if no refresh occured between indexiing and refresh, GET API will access the transaction log to fetch the document. however , some fields are generated only when indexing . if you try to access a field that is only generated when indexing , you will get an exception
you can choose to ignore field that are generated if the transaction log is accessed by settting 
`ignore_errors_on_generated_fields=true`
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST "http://localhost:9200/twitter/tweet?pretty=true" -d '{"user" : "xiaohu-liu","post_date" : "2009-11-15T14:12:12"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVweZq30B3_RrgjQvGlU",
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


## Getting the _source directly
use the `/{index}/{type}/{id}/_source` endpoint to get just the `_source` field of the document, without any additional content around it. for example as follows:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET "http://localhost:9200/twitter/tweet/AVweZq30B3_RrgjQvGlU/_source?pretty=true"
{
  "user" : "xiaohu-liu",
  "post_date" : "2009-11-15T14:12:12"
}
```
at the same time , you can also use source filtering parameters to control which parts of the `_source` will be returned:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET "http://localhost:9200/twitter/tweet/AVweZq30B3_RrgjQvGlU/_source?pretty=true&_source_include=*user"
{
  "user" : "xiaohu-liu"
}
```
<strong>Note: </strong> there is also a `HEAD`  variant for the _source endpoint to effeciently test for document _source existence.
```
[xiaohu-liu@cdh1 data-tmp]$ curl -i  -X HEAD "http://localhost:9200/twitter/tweet/AVweZq30B3_RrgjQvGlU/_source?pretty=true"
HTTP/1.1 200 OK
content-type: application/json; charset=UTF-8
content-length: 67
```

## Routing
When indexing using the ability to control the routing , in order to get the document , the routing value should also be provided . 
for example as follows:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST "http://localhost:9200/twitter/tweet?routing=xiaohu-liu1&pretty=true" -d '{"user" : "xiaohu-liu1","post_date" : "2009-11-15T14:12:12"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwedqYHB3_RrgjQvGlX",
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
let's use GET API to fetch it:

* with routing parameter setted
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET "http://localhost:9200/twitter/tweet/AVwedqYHB3_RrgjQvGlX?routing=xiaohu-liu1&pretty=true"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwedqYHB3_RrgjQvGlX",
  "_version" : 1,
  "_routing" : "xiaohu-liu1",
  "found" : true,
  "_source" : {
    "user" : "xiaohu-liu1",
    "post_date" : "2009-11-15T14:12:12"
  }
}
```
* without routing parameter setted
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET "http://localhost:9200/twitter/tweet/AVwedqYHB3_RrgjQvGlX?pretty=true"
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwedqYHB3_RrgjQvGlX",
  "found" : false
}
```

## Preference
By default, the operation is randomized between the shard replicas, and control a `preference` of which shard replicas to execute the get request. the optional values of the `preference` are `_primary`,`_local`,`Custom(string) value`, let's dig into the three value.
* _primary
   the operation will go and execute only on the primary shards.
* _local
   the operation will prefer to be executed on a local allocated shard if possible.
* Custom(string) value
   it can be something like the web session id or the user's name , since it will be used to guarantee the same shards will be used for the same custom value.
   
   




