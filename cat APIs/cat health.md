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

use a batch command to check the nodes health of the cluster:
```
% pssh -i -h list.of.cluster.hosts curl -s localhost:9200/_cat/health
[1] 20:20:52 [SUCCESS] es3.vm
1384309218 18:20:18 foo green 3 3 3 3 0 0 0 0
[2] 20:20:52 [SUCCESS] es1.vm
1384309218 18:20:18 foo green 3 3 3 3 0 0 0 0
[3] 20:20:52 [SUCCESS] es2.vm
1384309218 18:20:18 foo green 3 3 3 3 0 0 0 0
```
use a command to trace the recovery process:
```
% while true; do curl localhost:9200/_cat/health; sleep 120; done
1384309446 18:24:06 foo red 3 3 20 20 0 0 1812 0
1384309566 18:26:06 foo yellow 3 3 950 916 0 12 870 0
1384309686 18:28:06 foo yellow 3 3 1328 916 0 12 492 0
1384309806 18:30:06 foo green 3 3 1832 916 4 0 0
```




