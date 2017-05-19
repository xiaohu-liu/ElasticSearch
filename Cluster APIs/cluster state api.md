# cluster state api

The cluster state API allows to get a comprehensive state information of the whole cluster.
```
[xiaohu-liu@cdh1 ~]$ curl -XGET 'http://localhost:9200/_cluster/state?pretty'
```

## Response filter

As the cluster state can grow (depending on the number of shards and indices, your mapping, templates), it is possible to filter the cluster state response specifying the parts in the URL.
```
$ curl -XGET 'http://localhost:9200/_cluster/state/{metrics}/{indices}'
```

metrics can be a comma-separated list of
* `version`    Shows the cluster state version.
* `master_node`   Shows the elected master_node part of the response
* `nodes`     Shows the nodes part of the response
* `routing_table`     Shows the routing_table part of the response. If you supply a comma separated list of indices, the returned output will only contain the indices listed.
* `metadata`   Shows the metadata part of the response. If you supply a comma separated list of indices, the returned output will only contain the indices listed.
* `blocks`   Shows the blocks part of the response

for example here:
```
# return only metadata and routing_table data for specified indices
$ curl -XGET 'http://localhost:9200/_cluster/state/metadata,routing_table/foo,bar'

# return everything for these two indices
$ curl -XGET 'http://localhost:9200/_cluster/state/_all/foo,bar'

# Return only blocks data
$ curl -XGET 'http://localhost:9200/_cluster/state/blocks'
```
