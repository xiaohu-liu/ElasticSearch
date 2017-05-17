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
<strong>Note:</strong> the `ctx._source` is just the  source field of current document to update
<strong>Note:</strong> Now, updates can only be performed on a single document at a time, and in futuer , ElasticSearch will provide the ability to update multiple document given a query condition


