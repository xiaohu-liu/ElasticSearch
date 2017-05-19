# cat count
count provides quick access to the document count of the entire cluster, or individual indices.

```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/count?v'
epoch      timestamp count
1495179480 00:38:00  1041
```
<strong>Note: The document count indicates the number of live documents and does not include deleted documents which have not yet been cleaned up by the merge process.</strong>
