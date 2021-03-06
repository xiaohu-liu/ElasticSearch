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
 
 
 #### Match Query
 it can be thought of as a basic fielded search query 
 Let's take some examples as follow:
 
 
this call returns the account numbered 20
 ```
 GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}
 ```
this call returns all accounts containing the term "mill" in the address
 ```
 GET /bank/_search
{
  "query": { "match": { "address": "mill" } }
}
 ```
this call returns all accounts containing the term "mill" or "lane" in the address
 ```
 GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
 ```
this call  is a variant of match (match_phrase) that returns all accounts containing the phrase "mill lane" in the address:
```
GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
```

#### Bool(ean) Query
* must

this call returns all accounts containing "mill" and "lane" in the address
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
```
<strong>Note: </strong>the bool must clause specifies all the queries that must be true for a document to be considered a match

* should

this call returns all accounts containing "mill" or "lane" in the address
```
GET /bank/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
```
<strong>Note: </strong>the bool should clause specifies a list of queries either of which must be true for a document to be considered a match


* must_not
this call returns all accounts that contain neither "mill" nor "lane" in the address
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
```
<strong>Note: </strong> the bool must_not clause specifies a list of queries none of which must be true for a document to be considered a match

* mixed 

We can combine must, should, and must_not clauses simultaneously inside a bool query, Furthermore, we can compose bool queries inside any of these bool clauses to mimic any complex multi-level boolean logic.

this call returns all accounts of anybody who is 40 years old but doesn’t live in ID(aho)
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
```

### Filter Query
the _score field within the search result is just a relative measure of how well the documnet matches the search query that we specfied,the high socre and the more revelant the document is , the low score and the less revelant the document is

<strong>Note: </strong> the score is not neccessary to produce, in particular when they used for "filtering" the document set. in this case , ElasticSearch will automatically optimizes query execution in order not to compute useless score.

## Range filter
it is generally used for numeric or date filtering.

let's take the example as follow:

 we want to find accounts with a balance that is greater than or equal to 20000 and less than or equal to 30000
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```
<strong>Note: </strong>In addition to the match_all, match, bool, and range queries, there are a lot of other query types that are available.we will dig in them in the later phrase.



### Aggregation
Aggregations provide the ability to group and extract statistics from your data

let's take the example bellow to cover the aggregation
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}
```
In SQL, the call can be translated to  like this as follow:
```
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC
```

as for the response text:
```
{
  "took": 29,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "failed": 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "doc_count_error_upper_bound": 20,
      "sum_other_doc_count": 770,
      "buckets" : [ {
        "key" : "ID",
        "doc_count" : 27
      }, {
        "key" : "TX",
        "doc_count" : 27
      }, {
        "key" : "AL",
        "doc_count" : 25
      }, {
        "key" : "MD",
        "doc_count" : 25
      }, {
        "key" : "TN",
        "doc_count" : 23
      }, {
        "key" : "MA",
        "doc_count" : 21
      }, {
        "key" : "NC",
        "doc_count" : 21
      }, {
        "key" : "ND",
        "doc_count" : 21
      }, {
        "key" : "ME",
        "doc_count" : 20
      }, {
        "key" : "MO",
        "doc_count" : 20
      } ]
    }
  }
}
```
We can see that there are 27 accounts in ID (Idaho), followed by 27 accounts in TX (Texas), followed by 25 accounts in AL (Alabama), and so forth.

## How to get the average with some conditions specified
let's take an example to show that:
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```
You can nest aggregations inside aggregations arbitrarily to extract pivoted summarizations that you require from your data.

let’s now sort on the average balance in descending order
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```

This example demonstrates how we can group by age brackets (ages 20-29, 30-39, and 40-49), then by gender, and then finally get the average account balance, per age bracket, per gender:
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}
```

and there are so many other aggregations capabilities that we will go into detail about them in the later phrases.

