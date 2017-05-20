# task manager api

The task management API allows to retrieve information about the tasks currently executing on one or more nodes in the cluster.
for example:
```
curl -XGET http://localhost:9200/_tasks
curl -XGET http://localhost:9200/_tasks?nodes=nodeId1,nodeId2 ##　
curl -XGET http://localhost:9200/_tasks?nodes=nodeId1,nodeId2&actions=cluster:* 
```
the response text is just like this:
```
[root@cdh1 ~]# curl -XGET 'http://localhost:9200/_tasks?pretty&detailed&nodes=cdh1'
{
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39:9300",
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "tasks" : {
        "-foTUedtRtuy95CQ0UoLwA:390" : {
          "node" : "-foTUedtRtuy95CQ0UoLwA",
          "id" : 390,
          "type" : "transport",
          "action" : "cluster:monitor/tasks/lists",
          "description" : "",
          "start_time_in_millis" : 1495240567081,
          "running_time_in_nanos" : 766509,
          "cancellable" : false
        },
        "-foTUedtRtuy95CQ0UoLwA:391" : {
          "node" : "-foTUedtRtuy95CQ0UoLwA",
          "id" : 391,
          "type" : "direct",
          "action" : "cluster:monitor/tasks/lists[n]",
          "description" : "",
          "start_time_in_millis" : 1495240567082,
          "running_time_in_nanos" : 353781,
          "cancellable" : false,
          "parent_task_id" : "-foTUedtRtuy95CQ0UoLwA:390"
        }
      }
    }
  }
}
```

It is also possible to retrieve information for a particular task:
```
curl -X GET http://localhost:9200/_tasks/task_id:<task_id_value>
```
Or to retrieve all children of a particular task:
```
curl -X GET http://localhost:9200/_tasks?parent_task_id=parentTaskId:1
```

The task API can also be used to wait for completion of a particular task. 
```
curl -X GET http://localhost:9200/_tasks/oTUltX4IQMOUUVeiohTt8A:12345?wait_for_completion=true&timeout=10s
```

You can also wait for all tasks for certain action types to finish
```
curl -X GET http://localhost:9200/_tasks?actions=*reindex&wait_for_completion=true&timeout=10s
```
tasks can be also listed using _cat version of the list tasks command, which accepts the same arguments as the standard list tasks command.
```
curl -X GET http://localhost:9200/_cat/tasks
curl -X GET http://localhost:9200/_cat/tasks?detailed
```

## task cancellation
If a long-running task supports cancellation, it can be cancelled by the following command:
```
curl -X POST http://localhost:9200/_tasks/node_id:task_id/_cancel
```
multiple tasks can be cancelled at the same time
```
curl -X POST http://localhost:9200/_tasks/_cancel?nodes=nodeId1,nodeId2&actions=*reindex
```

## task group
The task lists returned by task API commands can be grouped either by nodes (default) or by parent tasks using the `group_by` parameter
```
curl -X GET http://localhost:9200/_tasks?group_by=parents
```
