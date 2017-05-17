### Explore data
let’s try to work on a more realistic dataset and you can download the file from `https://github.com/xiaohu-liu/ElasticSearch/blob/master/Data%20sample/accounts.json`

The data structure:
```
{
    "account_number": 0,
    "balance": 16623,
    "firstname": "Bradshaw",
    "lastname": "Mckenzie",
    "age": 29,
    "gender": "F",
    "address": "244 Columbus Place",
    "employer": "Euron",
    "email": "bradshawmckenzie@euron.com",
    "city": "Hobucken",
    "state": "CO"
}
```
### Load the sample data
```
curl -H "Content-Type: application/json" -XPOST 'localhost:9200/bank/account/_bulk?pretty&refresh' --data-binary "@accounts.json"
curl 'localhost:9200/_cat/indices?v'
```
the repsonse text as follow:
```
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank  l7sSYV2cQXmu6_4rJWVIww   5   1       1000            0    128.6kb        128.6kb
```

Which means that we just successfully bulk indexed 1000 documents into the bank index (under the account type).
### Query API

2 ways to start the query: one is by sending search parameters through the `REST request URI` and the other by sending them through the `REST request body`

REST request URI
```
GET twitter/tweet/_search?q=user:kimchy
```
response text as follow:
```
{
    "timed_out": false,
    "took": 62,
    "_shards":{
        "total" : 1,
        "successful" : 1,
        "failed" : 0
    },
    "hits":{
        "total" : 1,
        "max_score": 1.3862944,
        "hits" : [
            {
                "_index" : "twitter",
                "_type" : "tweet",
                "_id" : "0",
                "_score": 1.3862944,
                "_source" : {
                    "user" : "kimchy",
                    "date" : "2009-11-15T14:12:12",
                    "message" : "trying out Elasticsearch",
                    "likes": 0
                }
            }
        ]
    }
}
```
REST request body
```
GET /twitter/tweet/_search
{
    "query" : {
        "term" : { "user" : "kimchy" }
    }
}
```
response text as follow:
```
{
    "took": 1,
    "timed_out": false,
    "_shards":{
        "total" : 1,
        "successful" : 1,
        "failed" : 0
    },
    "hits":{
        "total" : 1,
        "max_score": 1.3862944,
        "hits" : [
            {
                "_index" : "twitter",
                "_type" : "tweet",
                "_id" : "0",
                "_score": 1.3862944,
                "_source" : {
                    "user" : "kimchy",
                    "message": "trying out Elasticsearch",
                    "date" : "2009-11-15T14:12:12",
                    "likes" : 0
                }
            }
        ]
    }
}
```

The REST API for search is accessible from the `_search` endpoint
for example as follow:
```
GET /bank/_search?q=*&sort=account_number:asc&pretty
```
the call above to return all the document in the blank index.
* q=* parameter instructs Elasticsearch to match all documents in the index
* sort=account_number:asc parameter indicates to sort the results using the account_number field of each document in an ascending order
* pretty parameter to specify the output in json format

the response text as follow:
```
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```
let's see the following parts in the response
* took - time in milliseconds for es to execute the query
* time_out - tell us the search is timeout or not
* _shards - tell us how many shards were searched , as well as the a count of the successful/failed searched shards\
* hits – search results
* hits.total – total number of documents matching our search criteria
* hits.hits – actual array of search results (defaults to first 10 documents)
* hits.sort - sort key for results (missing if sorting by score)
* hits._score and max_score - ignore these fields for now

okay, for the same search call, how we should do if we choose to use the `REST request body`
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

<strong>Note:</strong> ElasticSearch won't hold the status of the pre-request and treat every request fairly.

### Excecuting Search
 Let’s dig in some more into the Query DSL, by default, the full documents will return as part of all searches
 
 How show we do if we just want some fields but not the entire source document?
 
 The example as follow shows what we should step for that.
 ```
 GET /bank/_search
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}
 ```
 It will still only return one field named _source but within it, only the fields account_number and balance are included.
 
 



