# Update API
the update api allow you to update the document with a script provided.

process flow:
* get the document from the index specified
* run the script 
* reindex back the result

it uses the versioning to make sure no updates have happened during the `get` and `reindex`, for example here
```
PUT test/type1/1
{
    "counter" : 1,
    "tags" : ["red"]
}
```

## Scripted updates
Now, we can execute a script that would increment the counter
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfOXKDB3_RrgjQvGlh/_update?pretty=true' -d '{"script":{"inline":"ctx._source.age += params.count", "lang":"painless","params":{"count":20}}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfOXKDB3_RrgjQvGlh",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```
let's retrieve the document with id `AVwfOXKDB3_RrgjQvGlh` again to see the change occurs:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfOXKDB3_RrgjQvGlh?pretty=true'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfOXKDB3_RrgjQvGlh",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 30
  }
}
```

We can handle the array field within the document:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet?pretty' -d '{"username":"xiaohu-liu1","age":10, "hobbies":["running"]}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfQQXOB3_RrgjQvGl-",
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
retrieve the document with id `AVwfQQXOB3_RrgjQvGl`
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfQQXOB3_RrgjQvGl-?pretty=true'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfQQXOB3_RrgjQvGl-",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 10,
    "hobbies" : [
      "running"
    ]
  }
}
```

update the `hobbies` field like this as follows:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfQQXOB3_RrgjQvGl-?pretty=true' -d '{"script":{"inline":"ctx._source.hobbies.add(params.favor)", "lang":"painless","params":{"favor":"jumping"}}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfQQXOB3_RrgjQvGl-",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "created" : false
}
```
retrieve the document again:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfQQXOB3_RrgjQvGl-?pretty=true'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfQQXOB3_RrgjQvGl-",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 10,
    "hobbies" : [
      "running",
      "jumping"
    ]
  }
}
```
in addition to `_source`, the following variables are available through the `ctx` map: `_index`,`_type`,`_version`,`_id`,`_routing`,`_parent`,`_now`.


we can also add a new field to the document:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB/_update?pretty' -d '{"script":"ctx._source.timestamp=ctx._now"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```
retrieve the document with id `AVwfSTIUB3_RrgjQvGmB`
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB?pretty=true'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 10,
    "hobbies" : [
      "running"
    ],
    "timestamp" : 1495173622364
  }
}
```

remove a field from the document
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB/_update?pretty=true' -d '{"script":"ctx._source.remove(\"username\")"}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB?pretty=true'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 3,
  "found" : true,
  "_source" : {
    "age" : 10,
    "hobbies" : [
      "running"
    ],
    "timestamp" : 1495173622364
  }
}
```
the `username` field was removed from the document

And we can even change the type of operation that is executed, This exmaple as follows deletes the doc if the `hobbies` field contains `jumping` , otherwise it does nothing
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB/_update?pretty' -d '{"script":{"inline":"if (ctx._source.hobbies.contains(params.favor)) {ctx.op = \"delete\"} else {ctx.op = \"none\"}", "lang":"painless","params":{"favor":"jumping"}}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 3,
  "result" : "noop",
  "_shards" : {
    "total" : 0,
    "successful" : 0,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfSTIUB3_RrgjQvGmB/_update?pretty' -d '{"script":{"inline":"if (ctx._source.hobbies.contains(params.favor)) {ctx.op = \"delete\"} else {ctx.op = \"none\"}", "lang":"painless","params":{"favor":"jumping"}}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfSTIUB3_RrgjQvGmB",
  "_version" : 5,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```
 




