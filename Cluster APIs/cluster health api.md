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
