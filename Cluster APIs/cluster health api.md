# cluster health api

he cluster health API allows to get a very simple status on the health of the cluster
```
[xiaohu-liu@cdh1 ~]$ curl -XGET 'http://localhost:9200/_cluster/health?pretty'
{
  "cluster_name" : "xiaohu-liu",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 25,
  "active_shards" : 25,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 25,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}

```

The API can also be executed against one or more indices to get just the specified indices health:
```
[xiaohu-liu@cdh1 ~]$ curl -XGET 'http://localhost:9200/_cluster/health/bank?pretty'
{
  "cluster_name" : "xiaohu-liu",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 5,
  "active_shards" : 5,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 5,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}

```
health status details:
* `red` shard is not allocated in the cluster
* `yellow`  primary shard is allocated but replicas are not
* `green` all shards are allocated

<strong>Note: </strong>
The index level status is controlled by the worst shard status. The cluster status is controlled by the worst index status.

the parameter of cluster health api:
* `level`  Can be one of cluster, indices or shards.
* `wait_for_status` One of green, yellow or red. won't wait for any status by default.
* `wait_for_no_relocating_shards`  won't wait for any  replicating shard by default.
* `wait_for_active_shards` all or 0 , 0 by default.
* `wait_for_nodes` The request waits until the specified number N of nodes is available
* `timeout` how long to wait if one of the wait_for_XXX are provided, default 30s
* `local`  If true returns the local node information and does not provide the state from master node. Default: false.



  
