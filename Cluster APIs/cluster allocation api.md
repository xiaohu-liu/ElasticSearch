# cluster allocation api

The purpose of the cluster allocation explain API is to provide explanations for shard allocations in the cluster. 
* For unassigned shards, the explain API provides an explanation for why the shard is unassigned
* For assigned shards, the explain API provides an explanation for why the shard is remaining on its current node and has not moved or rebalanced to another node

This API can be very useful when attempting to diagnose why a shard is unassigned or why a shard continues to remain on its current node when you might expect otherwise.


here is an example:
```
curl -X PUT 'http://localhost:9200/myindex'
[root@cdh1 ~]# curl -XGET 'http://localhost:9200/_cluster/allocation/explain?pretty'
{
  "index" : "myindex",
  "shard" : 3,
  "primary" : false,
  "current_state" : "unassigned",
  "unassigned_info" : {
    "reason" : "INDEX_CREATED",
    "at" : "2017-05-20T01:04:41.437Z",
    "last_allocation_status" : "no_attempt"
  },
  "can_allocate" : "no",
  "allocate_explanation" : "cannot allocate because allocation is not permitted to any of the nodes",
  "node_allocation_decisions" : [
    {
      "node_id" : "-foTUedtRtuy95CQ0UoLwA",
      "node_name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "node_decision" : "no",
      "weight_ranking" : 1,
      "deciders" : [
        {
          "decider" : "same_shard",
          "decision" : "NO",
          "explanation" : "the shard cannot be allocated to the same node on which a copy of the shard already exists [[myindex][3], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=Xke6k26KTUaBfkA3FoIm3g]]"
        }
      ]
    }
  ]
}
```

## include_disk_info 
this url parameter to be setted to  return information gathered by the cluster info service about disk usage and shard sizes .

## include_yes_decisions 
this url parameter to be setted to return the response  including all decisions that were factored into the final decision.

```
[root@cdh1 ~]# curl -XGET 'http://localhost:9200/_cluster/allocation/explain?pretty&include_disk_info=true&include_yes_decisions=true'
{
  "index" : "myindex",
  "shard" : 3,
  "primary" : false,
  "current_state" : "unassigned",
  "unassigned_info" : {
    "reason" : "INDEX_CREATED",
    "at" : "2017-05-20T01:04:41.437Z",
    "last_allocation_status" : "no_attempt"
  },
  "cluster_info" : {
    "nodes" : {
      "-foTUedtRtuy95CQ0UoLwA" : {
        "node_name" : "cdh1",
        "least_available" : {
          "path" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
          "total_bytes" : 18579001344,
          "used_bytes" : 5486354432,
          "free_bytes" : 13092646912,
          "free_disk_percent" : 70.5,
          "used_disk_percent" : 29.5
        },
        "most_available" : {
          "path" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
          "total_bytes" : 18579001344,
          "used_bytes" : 5486354432,
          "free_bytes" : 13092646912,
          "free_disk_percent" : 70.5,
          "used_disk_percent" : 29.5
        }
      }
    },
    "shard_sizes" : {
      "[myindex][2][p]_bytes" : 4342,
      "[myindex][1][p]_bytes" : 130,
      "[myindex][0][p]_bytes" : 130,
      "[myindex][3][p]_bytes" : 130,
      "[myindex][4][p]_bytes" : 130
    },
    "shard_paths" : {
      "[myindex][2], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=8qJ9eyLqTtS0A5EcrzjPgQ]" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
      "[myindex][4], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=2CcPZmYOQ0eQWPkiNr8HBw]" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
      "[myindex][1], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=ysHVkEyRRzivLQKYhQ_6jA]" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
      "[myindex][3], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=Xke6k26KTUaBfkA3FoIm3g]" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
      "[myindex][0], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=KLy0D3WAR-CvbZx7J_GnLg]" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0"
    }
  },
  "can_allocate" : "no",
  "allocate_explanation" : "cannot allocate because allocation is not permitted to any of the nodes",
  "node_allocation_decisions" : [
    {
      "node_id" : "-foTUedtRtuy95CQ0UoLwA",
      "node_name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "node_decision" : "no",
      "weight_ranking" : 1,
      "deciders" : [
        {
          "decider" : "max_retry",
          "decision" : "YES",
          "explanation" : "shard has no previous failures"
        },
        {
          "decider" : "replica_after_primary_active",
          "decision" : "YES",
          "explanation" : "primary shard for this replica is already active"
        },
        {
          "decider" : "enable",
          "decision" : "YES",
          "explanation" : "all allocations are allowed"
        },
        {
          "decider" : "node_version",
          "decision" : "YES",
          "explanation" : "target node version [5.4.0] is the same or newer than source node version [5.4.0]"
        },
        {
          "decider" : "snapshot_in_progress",
          "decision" : "YES",
          "explanation" : "the shard is not being snapshotted"
        },
        {
          "decider" : "filter",
          "decision" : "YES",
          "explanation" : "node passes include/exclude/require filters"
        },
        {
          "decider" : "same_shard",
          "decision" : "NO",
          "explanation" : "the shard cannot be allocated to the same node on which a copy of the shard already exists [[myindex][3], node[-foTUedtRtuy95CQ0UoLwA], [P], s[STARTED], a[id=Xke6k26KTUaBfkA3FoIm3g]]"
        },
        {
          "decider" : "disk_threshold",
          "decision" : "YES",
          "explanation" : "there is only a single data node present"
        },
        {
          "decider" : "throttling",
          "decision" : "YES",
          "explanation" : "below shard recovery limit of outgoing: [0 < 2] incoming: [0 < 2]"
        },
        {
          "decider" : "shards_limit",
          "decision" : "YES",
          "explanation" : "total shard limits are disabled: [index: -1, cluster: -1] <= 0"
        },
        {
          "decider" : "awareness",
          "decision" : "YES",
          "explanation" : "allocation awareness is not enabled, set cluster setting [cluster.routing.allocation.awareness.attributes] to enable it"
        }
      ]
    }
  ]
}
```
