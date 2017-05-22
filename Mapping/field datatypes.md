# field datatypes

## Array DataType
there is not dedicated `array` type in es. any field can contains 0 or multiple values by default.
however , all values in the same array must be of same datatype. for instance:
```
an array of strings: [ "one", "two" ]
an array of integers: [ 1, 2 ]
an array of arrays: [ 1, [ 2, 3 ]] which is the equivalent of [ 1, 2, 3 ]
an array of objects: [ { "name": "Mary", "age": 12 }, { "name": "John", "age": 10 }]
```
<strong>Note: </strong>
* the first value in the array determines the field type when adding a field dynamically
* all subsequent values must be of the same datatype or it must at least be possible to coerce subsequent values to the same datatype
* an array can contains null value
* an empty array [] is treated as a missing field — a field with no values
* it's not necessary to pre-configure in the document to use array, as it's out of the box.

## Binary DataType
The binary type accepts a binary value as a Base64 encoded string. </br>
The field is not stored by default and is not searchable:
```
PUT es-index
{
  "mappings": {
    "my_type": {
      "properties": {
        "name": {
          "type": "text"
        },
        "blob": {
          "type": "binary"
        }
      }
    }
  }
}

PUT es-index/es-type/1
{
  "name": "Some binary blob",
  "blob": "U29tZSBiaW5hcnkgYmxvYg==" 
}
```
## Range DataTypes
The following range types are supported:
* integer range (32bit)
* float range(32 bit)
* long range(64 bit)
* double range(64 bit)
* date_range (unsigned 64-bit integer milliseconds)

for example as follows:
```
@range_type_data.json
{
  "expected_attendees" : { 
    "gte" : 10,
    "lte" : 20
  },
  "time_frame" : { 
    "gte" : "2015-10-31 12:00:00", 
    "lte" : "2015-11-01"
  }
}

@range_type.json
{
  "mappings": {
    "my_type": {
      "properties": {
        "expected_attendees": {
          "type": "integer_range"
        },
        "time_frame": {
          "type": "date_range", 
          "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
        }
      }
    }
  }
}

create the index named `es-index3`
[xiaohu-liu@cdh1 json]$ curl -X PUT 'http://localhost:9200/es-index3?pretty' -d "@range_type.json"
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}

@range_type_query.json
{
  "query" : {
    "range" : {
      "time_frame" : { 
        "gte" : "2015-10-31",
        "lte" : "2015-11-01",
        "relation" : "within" 
      }
    }
  }
}


index the document given:
[xiaohu-liu@cdh1 json]$ curl -X PUT 'http://localhost:9200/es-index3/my_type/1?pretty' -d "@range_type_data.json"
{
  "_index" : "es-index3",
  "_type" : "my_type",
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

search the target document:
$ curl -X POST 'http://localhost:9200/es-index3/_search?pretty' -d "@range_type_query.json"
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "es-index3",
        "_type" : "my_type",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "expected_attendees" : {
            "gte" : 10,
            "lte" : 20
          },
          "time_frame" : {
            "gte" : "2015-10-31 12:00:00",
            "lte" : "2015-11-01"
          }
        }
      }
    ]
  }
}

```
## Boolean DataType

Boolean fields accept JSON true and false values, but can also accept strings and numbers which are interpreted as either true or false:

* False values  false, "false", "off", "no", "0", "" (empty string), 0, 0.0
* Anything that isn’t false.

## Date DataType
JSON doesn’t have a date datatype, so dates in Elasticsearch can either be:
* strings containing formatted dates, e.g. "2015-01-01" or "2015/01/01 12:10:30".
* a long number representing milliseconds-since-the-epoch.
* an integer representing seconds-since-the-epoch.

Internally, dates are converted to UTC (if the time-zone is specified) and stored as a long number representing milliseconds-since-the-epoch.

Date formats can be customised, but if no format is specified then it uses the default:
```
"strict_date_optional_time||epoch_millis"
```
This means that it will accept dates with optional timestamps, which conform to the formats supported by strict_date_optional_time or milliseconds-since-the-epoch.


Multiple formats can be specified by separating them with || as a separator. Each format will be tried in turn until a matching format is found. The first format will be used to convert the milliseconds-since-the-epoch value back into a string.

for example here:
```
PUT my_index
{
  "mappings": {
    "my_type": {
      "properties": {
        "date": {
          "type":   "date",
          "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
        }
      }
    }
  }
}
```
