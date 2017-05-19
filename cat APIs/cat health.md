# cat health
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/health?v'
epoch      timestamp cluster    status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1495179925 00:45:25  xiaohu-liu yellow          1         1     25  25    0    0       25             0                  -                 50.0%
```

It has one option ts to disable the timestamping:
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/health?v&ts=false'
cluster    status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
xiaohu-liu yellow          1         1     25  25    0    0       25             0                  -                 50.0%
```


