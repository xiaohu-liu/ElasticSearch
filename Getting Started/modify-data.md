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


