# Delete By Query API
the usage of `_delete_by_query` performs a deletion on every document that match a query. for example as follow:
```
[xiaohu-liu@cdh1 data-tmp]$ curl -X POST 'http://localhost:9200/twitter/_delete_by_query?pretty' {d '{"query":{"match":{"user":"xiaohu-liu"}}}'
  "took" : 555,
  "timed_out" : false,
  "total" : 6,
  "deleted" : 6,
  "batches" : 1,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}
```
* the `query` keyword is must be passed.
* you can use `q` parameter in the same way as the search api
* `_delete_by_query` gets a snapshot of the index when it starts and deletes what it finds using `internal` versioning. you may get a version conflict if the document changes between the time when the snapshot was token and when the delete request is actually processed. when the versions match the document is deleted.

<strong>Note: </strong> Since `internal` versioning does not support the value 0 as a valid versin number. documents with version equal to 0 cannot be deleted using `_delete_by_query` and will fail the request.

let's take a look at the field named `failures`, during the `_delete_by_query` execution, multiple search will processed sequentially in order to find all the matching document to delete. every time a batch of documents is found, a corresponding bulk request is executed to delete all these documents. in case a search or a bulk request got rejected. `_delete_by_query` relies on a default policy to retry rejected request(up to 10 times), reaching the maximum retries limit causes the `_delete_by_query` to abort and all failure are returned in the `failures` of the response. and the process is not rolled back, only aborted. 

while the first failure causes the abort, all failures that are returned by the failing bulk requestt are returned in the `failures` element; so there it's possible for there to be quite a few failed entities.

* how to count the version conflicts but not abort the process
    * set `conflicts=proceed` on the url
    * set `conflicts:process` in the request body
    
you can limit `_delete_by_query` to a single type not only the entire types of the index specified. and this will only delete the specified type document of the index. for example here:
```
POST twitter/tweet/_delete_by_query?conflicts=proceed
{
  "query": {
    "match_all": {}
  }
}
```

it's possible to delete documents of multiple indexes and multiple types at once , just like as follows:
```
POST twitter,blog/tweet,post/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

if you provide `routing` then the routing is copied to the scroll query, limiting the processing to the shards that match that routing value. for example:
```
POST twitter/_delete_by_query?routing=1
{
  "query": {
    "range" : {
        "age" : {
           "gte" : 10
        }
    }
  }
}
```
the batch size of scroll query is 1000 by default , and you can change the batch size with the `scroll_size` url parameter.
```
POST twitter/_delete_by_query?scroll_size=5000
{
  "query": {
    "term": {
      "user": "kimchy"
    }
  }
}
```
## URL Parameters
in addition to the parameters like `pretty`, the delete by query api also support `refresh`, `wait_for_completion`,`wait_for_active_shard` and `timeout`

### refresh
* sending the `refresh` will refresh all the shards involved in the delete by query once the request completes.
* refresh the shard that received the delete request in the Delete API.

### wait_for_completion = false
if the request contains `wait_for_completion=false` then es will perform some preflight check, lauch the request, and then return the `task` which can be used with TaskAPI to cancel or get the status of the task. es will also create a record of this task as a document at `.tasks/task/${taskId}` , and you can keep or remove as you see fit, when you are done with it , delete it so es can reclaim the space it uses.

### wait_for_active_shards 
 it controls how many copies of shard must be active before actual proceeding with the request.
 
### timeout
it control how long each write request waits for unavailable shards to beacome available.


### request_per_second 
//wait for adding 

## Response Body
```
{
  "took" : 639,
  "deleted": 0,
  "batches": 1,
  "version_conflicts": 2,
  "retries": 0,
  "throttled_millis": 0,
  "failures" : [ ]
}
```
* `took` 
    the millisecond from start to end of the whole operation
* `deleted`
    the number of documents that were successfully deleted
* `batches`
    the numner of scroll responses pulled back by the delete by query
* `version_conflicts`
    the number of version conflicts that the delete by query hit
* `retries`
    the number of retries that the delete by query didi in response to full queue
* `throttled_millis`
    the number of milliseconds request slept to conform to `request_per_second`
* `failures`
   Array of all indexing failures, if this is non-empty then the request aborted because of those failures.










