# cat allocation

allocation provides a snapshot of how many shards are allocated to each data node and how much disk space they are using.
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/allocation?v'
shards disk.indices disk.used disk.avail disk.total disk.percent host         ip           node
    25      782.2kb     4.5gb     12.7gb     17.3gb           26 10.100.0.146 10.100.0.146 cdh1
```
