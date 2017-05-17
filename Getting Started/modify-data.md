### Modify the data
ElasticSearch provides data manipulation and search capabilities in near real time, 1 second latency by default

<strong>Index and Update</strong>
Let's recall the command to index a document as follow:
```
PUT /customer/external/1?pretty
{
  "name": "John Doe"
}

```

okay , the document above will be indexed into `customer` index , `external` type , and with id of 1
if we call the command again with different document
```
PUT /customer/external/1?pretty
{
  "name": "Jane Doe"
}

```

* the document with id of 1 will be updated , the above will change the field named `name` from `John Doe` to `Jane Doe`
* with a different id , a new document will be indexed
* the id of the document to index is not neccessary to specify, elasticsearch will automatically generate a random id for it if there is 
not a explicit id specified, and then use it to index the doc, just as follow:
```
POST /customer/external?pretty
{
  "name": "Jane Doe"
}
```
<strong>Note:</strong>we are using the `POST` verb instead of `PUT` since we didn’t specify an ID.


### Update the document
Delete the old one and then index the new one with the update applied to it
for example as follow:
* Change the `name` field to `Jane Doe` 
```
POST /customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}
```
* Change the `name` field to `Jane Doe` and then add the `age` field to it at the same time
```
POST /customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
```
* use the script to update(`increse the age by 5`)
```
POST /customer/external/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```
* <strong>Note:</strong> the `ctx._source` is just the  source field of current document to update
* <strong>Note:</strong> Now, updates can only be performed on a single document at a time, and in future , ElasticSearch will provide the ability to update multiple document given a query condition


### Delete the document
Deleting a document is fairly straightforward
the example as follow shows how to delete the document with the id of 2
```
DELETE /customer/external/2?pretty
```
It is worth noting that it is much more efficient to delete a whole index instead of deleting all documents with the Delete By Query API.

### Batch Process
<strong>_bulk API.</strong>
Elasticsearch is able to index , update, delete the document and provides the ability to perform any of them in batch, the mechanism is 
fast and efficient with fewer network roudtrips.
```
POST /customer/external/_bulk?pretty
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
```
the example above indexes 2 documents in one bulk operation.

```
POST /customer/external/_bulk?pretty
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```
the example above updates 2 document with different id in one bulk operation

<strong>Note:</strong> The Bulk API does not fail due to failures in one of the actions

