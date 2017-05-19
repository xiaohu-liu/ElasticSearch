# cat shards

The shards command is the detailed view of what nodes contain which shards. It will tell you if it’s a primary or replica, the number of docs, the bytes it takes on disk, and the node where it’s located.

Here we see a single index, with one primary shard and no replicas:
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/shards?v'
index      shard prirep state      docs   store ip           node
bank       3     p      STARTED     200 128.1kb 10.100.0.146 cdh1
bank       3     r      UNASSIGNED                           
bank       4     p      STARTED     201 136.6kb 10.100.0.146 cdh1
bank       4     r      UNASSIGNED                           
bank       1     p      STARTED     191 122.8kb 10.100.0.146 cdh1
bank       1     r      UNASSIGNED                           
bank       2     p      STARTED     211 142.9kb 10.100.0.146 cdh1
bank       2     r      UNASSIGNED                           
bank       0     p      STARTED     197 126.5kb 10.100.0.146 cdh1
bank       0     r      UNASSIGNED                           
blogs      3     p      STARTED       0    130b 10.100.0.146 cdh1
blogs      3     r      UNASSIGNED                           
blogs      4     p      STARTED       0    130b 10.100.0.146 cdh1
blogs      4     r      UNASSIGNED                           
blogs      1     p      STARTED       1   3.7kb 10.100.0.146 cdh1
blogs      1     r      UNASSIGNED                           
blogs      2     p      STARTED       0    130b 10.100.0.146 cdh1
blogs      2     r      UNASSIGNED                           
blogs      0     p      STARTED       0    130b 10.100.0.146 cdh1
blogs      0     r      UNASSIGNED                           
twitter2   3     p      STARTED       0    159b 10.100.0.146 cdh1
twitter2   3     r      UNASSIGNED                           
twitter2   4     p      STARTED       0    130b 10.100.0.146 cdh1
twitter2   4     r      UNASSIGNED                           
twitter2   1     p      STARTED       0    130b 10.100.0.146 cdh1
twitter2   1     r      UNASSIGNED                           
twitter2   2     p      STARTED       0    130b 10.100.0.146 cdh1
twitter2   2     r      UNASSIGNED                           
twitter2   0     p      STARTED       0    130b 10.100.0.146 cdh1
twitter2   0     r      UNASSIGNED                           
twitter    3     p      STARTED       9    32kb 10.100.0.146 cdh1
twitter    3     r      UNASSIGNED                           
twitter    4     p      STARTED       8  30.1kb 10.100.0.146 cdh1
twitter    4     r      UNASSIGNED                           
twitter    1     p      STARTED       7  25.6kb 10.100.0.146 cdh1
twitter    1     r      UNASSIGNED                           
twitter    2     p      STARTED      10   8.9kb 10.100.0.146 cdh1
twitter    2     r      UNASSIGNED                           
twitter    0     p      STARTED       5  19.3kb 10.100.0.146 cdh1
twitter    0     r      UNASSIGNED                           
xiaohu-liu 3     p      STARTED       1   3.5kb 10.100.0.146 cdh1
xiaohu-liu 3     r      UNASSIGNED                           
xiaohu-liu 4     p      STARTED       0    130b 10.100.0.146 cdh1
xiaohu-liu 4     r      UNASSIGNED                           
xiaohu-liu 1     p      STARTED       0    130b 10.100.0.146 cdh1
xiaohu-liu 1     r      UNASSIGNED                           
xiaohu-liu 2     p      STARTED       0    130b 10.100.0.146 cdh1
xiaohu-liu 2     r      UNASSIGNED                           
xiaohu-liu 0     p      STARTED       0    130b 10.100.0.146 cdh1
xiaohu-liu 0     r      UNASSIGNED  
```

## index pattern
If you have many shards, you may wish to limit which indices show up in the output. You can always do this with grep, but you can save some bandwidth by supplying an index pattern to the end.
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/shards/bank?v'
index shard prirep state      docs   store ip           node
bank  3     p      STARTED     200 128.1kb 10.100.0.146 cdh1
bank  3     r      UNASSIGNED                           
bank  4     p      STARTED     201 136.6kb 10.100.0.146 cdh1
bank  4     r      UNASSIGNED                           
bank  1     p      STARTED     191 122.8kb 10.100.0.146 cdh1
bank  1     r      UNASSIGNED                           
bank  2     p      STARTED     211 142.9kb 10.100.0.146 cdh1
bank  2     r      UNASSIGNED                           
bank  0     p      STARTED     197 126.5kb 10.100.0.146 cdh1
bank  0     r      UNASSIGNED
```
and you can specify the heads you want the call to return, for example here:
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/shards/bank?v&h=index,shard,prirep,docs'
index shard prirep docs
bank  3     p       200
bank  3     r          
bank  4     p       201
bank  4     r          
bank  1     p       191
bank  1     r          
bank  2     p       211
bank  2     r          
bank  0     p       197
bank  0     r         
```

