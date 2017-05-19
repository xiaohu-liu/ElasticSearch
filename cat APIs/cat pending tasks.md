# cat pending tasks
pending_tasks provides the same information as the /_cluster/pending_tasks API in a convenient tabular format. For example:
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/pending_tasks?v'
insertOrder timeInQueue priority source
```
