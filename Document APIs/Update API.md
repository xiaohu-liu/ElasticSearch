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

## Updates with a partial document
the update api support passing a partial document , which will be merged into the existing document.
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE/_update?pretty' -d '{"doc":{"gender":"male"}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE?pretty'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 10,
    "hobbies" : [
      "running"
    ],
    "gender" : "male"
  }
}
```
if both `doc` and `script` are specified, then `doc` is ignored, Best is to put your field pairs of the partial document in the script itself.
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE/_update?pretty' -d '{"doc":{"gender1":"male"},"script":{"inline":"ctx._source.nickname=\"jspider\""}}'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "action_request_validation_exception",
        "reason" : "Validation Failed: 1: can't provide both script and doc;"
      }
    ],
    "type" : "action_request_validation_exception",
    "reason" : "Validation Failed: 1: can't provide both script and doc;"
  },
  "status" : 400
}
```

```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE/_update?pretty' -d '{"script":{"inline":"ctx._source.nickname=\"jspider\";ctx._source.birthday=\"1991-05-11\""}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE?pretty'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 3,
  "found" : true,
  "_source" : {
    "username" : "xiaohu-liu1",
    "age" : 10,
    "hobbies" : [
      "running"
    ],
    "gender" : "male",
    "nickname" : "jspider",
    "birthday" : "1991-05-11"
  }
}
```

## Detecting noop updates
update the document with the existing value in `_source`, by defaults updates that don't change anything detect that they don't change anything and return "result":"noop" like this as follows:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE/_update?pretty' -d '{"doc":{"nickname":"jspider","birthday":"1991-05-11"}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 4,
  "result" : "noop",
  "_shards" : {
    "total" : 0,
    "successful" : 0,
    "failed" : 0
  }
}
```
the update request was ignored, and you can disabled the detect by setting `detect_noop:false` like this:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE/_update?pretty' -d '{"doc":{"nickname":"jspider","birthday":"1991-05-11"},"detect_noop":false}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE",
  "_version" : 5,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
```
as we can see from the response above, the `result` field is `updated` rather than `noop` and the `_version` field has a automatic increment.


## Upserts
if the document does not already exist. the contents of the `upsert` element will be inserted as a new document. if the document does exist, then the `script` will be executed instead.
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/10/_update?pretty' -d '{"script":{"inline":"ctx._source.counter+= params.count", "lang":"painless","params":{"count":10}}, "upsert":{"counter":1}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "10",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/10/_update?pretty' -d '{"script":{"inline":"ctx._source.counter+= params.count", "lang":"painless","params":{"count":10}}, "upsert":{"counter":1}}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "10",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 data-tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/10?pretty'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "10",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "counter" : 11
  }
}
```
the `result` field is `created` in the first response text, and `updated` in the second response

## script_upsert
if you would like your script to run regardless of whatever the document exists or not- i.e the script handles initializing the document instead of `upsert` element -- then set the `scripted_upsert` to `true`.
```
[xiaohu-liu@cdh1 tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE1/_update?pretty' -d '@scripted_upsert.json'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}

scripted_upsert.json
{
    "scripted_upsert":true,
    "script":{
        "inline": "ctx._source.pageViewEvent=params.pageViewEvent",
        "lang":"painless",
        "params" : {
            "pageViewEvent" : {
                "url":"foo.com/bar",
                "response":404,
                "time":"2014-01-01 12:32"
            }
        }
    },
    "upsert" : {}
}
```
## doc_as_upsert
instead of sending a partial doc plus an `upsert` doc, setting `doc_as_upsert` to `true` will use the content of `doc` as the `upsert` value
```
[xiaohu-liu@cdh1 tmp]$ curl -X POST 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE6/_update?pretty' -d '{"doc":{"nickname":"jspider"},"doc_as_upsert":true}'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE6",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/twitter/tweet/AVwfVTFfB3_RrgjQvGmE6?pretty'
{
  "_index" : "twitter",
  "_type" : "tweet",
  "_id" : "AVwfVTFfB3_RrgjQvGmE6",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "nickname" : "jspider"
  }
}
```




