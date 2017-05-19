# cat nodes
The nodes command shows the cluster topology. for example
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/nodes?v'
ip           heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
10.100.0.146            8          82   0    0.00    0.00     0.00 mdi       *      cdh1
```
[there is an exhaustive list of the existing headers](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html) that can be passed to nodes?h= to retrieve the relevant details in ordered columns.
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/nodes?v&h=id,ip,port,v,m'
id   ip           port v     m
nwGP 10.100.0.146 9300 5.4.0 *
```

