# Node stats 

## Node statistics

The cluster nodes stats API allows to retrieve one or more (or all) of the cluster nodes statistics.
for example here:
```
curl -XGET 'http://localhost:9200/_nodes/stats'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/stats'
```
the first command retrieves stats of all nodes in the cluster, however the second command selectively retrieves stats of nodeId1 and nodeId2.
