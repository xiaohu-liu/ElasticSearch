# cat recovery
The recovery command is a view of index shard recoveries, both on-going and previously completed. It is a more compact view of the JSON recovery API.

A recovery event occurs anytime an index shard moves to a different node in the cluster. This can happen during a `snapshot recovery`, `a change in replication level`, `node failure`, or on `node startup`. This last type is called a local store recovery and is the normal way for shards to be loaded from disk when a node starts up.

for example:
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/recovery?v'
index      shard time  type           stage source_host source_node target_host  target_node repository snapshot files files_recovered files_percent files_total bytes bytes_recovered bytes_percent bytes_total translog_ops translog_ops_recovered translog_ops_percent
bank       0     219ms existing_store done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               100.0%        4           0     0               100.0%        129562      0            0                      100.0%
bank       1     242ms existing_store done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               100.0%        4           0     0               100.0%        125807      0            0                      100.0%
bank       2     235ms existing_store done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               100.0%        7           0     0               100.0%        146367      0            0                      100.0%
bank       3     285ms existing_store done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               100.0%        4           0     0               100.0%        131223      0            0                      100.0%
bank       4     59ms  existing_store done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               100.0%        7           0     0               100.0%        139968      0            0                      100.0%
twitter    0     97ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter    1     90ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter    2     62ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter    3     162ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter    4     24ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
xiaohu-liu 0     33ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
xiaohu-liu 1     62ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
xiaohu-liu 2     57ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
xiaohu-liu 3     87ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
xiaohu-liu 4     23ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
blogs      0     127ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
blogs      1     90ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
blogs      2     130ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
blogs      3     91ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
blogs      4     24ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter2   0     137ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter2   1     81ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter2   2     112ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter2   3     49ms  empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
twitter2   4     361ms empty_store    done  n/a         n/a         10.100.0.146 cdh1        n/a        n/a      0     0               0.0%          0           0     0               0.0%          0           0            0                      100.0%
```





