# Node stats 

## Node statistics

The cluster nodes stats API allows to retrieve one or more (or all) of the cluster nodes statistics.
for example here:
```
curl -XGET 'http://localhost:9200/_nodes/stats'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/stats'
```
the first command retrieves stats of all nodes in the cluster, however the second command selectively retrieves stats of nodeId1 and nodeId2.

you can control the result returned by specifying the metric.

optional metrics:
* `indices`
   * Indices stats about size, document count, indexing and deletion times, search times, field cache size, merges and flushes
* `fs`
   * File system information, data path, free disk space, read/write stats
* `http`
   * HTTP connection information
* `jvm`
   * JVM stats, memory pool information, garbage collection, buffer pools, number of loaded/unloaded classes
* `os`
   * Operating system stats, load average, mem, swap
* `process`
   * Process statistics, memory consumption, cpu usage, open file descriptors
* `thread_pool`
   * Statistics about each thread pool, including current size, queue and rejected tasks
* `transport`
   * Transport statistics about sent and received bytes in cluster communication
* `breaker`
   * Statistics about the field data circuit breaker
* `discovery`
   * Statistics about the discovery
* `ingest`
   * Statistics about ingest preprocessing
for example here
```
# return just indices
curl -XGET 'http://localhost:9200/_nodes/stats/indices'
# return just os and process
curl -XGET 'http://localhost:9200/_nodes/stats/os,process'
# return just process for node with IP address 10.0.0.1
curl -XGET 'http://localhost:9200/_nodes/10.0.0.1/stats/process'
```
<strong>Note: </strong> All stats can be explicitly requested via `/_nodes/stats/_all` or `/_nodes/stats?metric=_all`


## Fs Information
```
[root@cdh1 ~]# curl -X GET 'http://localhost:9200/_nodes/stats/fs?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "timestamp" : 1495238900560,
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39:9300",
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "fs" : {
        "timestamp" : 1495238900562,
        "total" : {
          "total_in_bytes" : 18579001344,
          "free_in_bytes" : 13820182528,
          "available_in_bytes" : 12869599232,
          "spins" : "true"
        },
        "data" : [
          {
            "path" : "/opt/elasticsearch/elasticsearch-5.4.0/data/nodes/0",
            "mount" : "/ (/dev/sda2)",
            "type" : "ext4",
            "total_in_bytes" : 18579001344,
            "free_in_bytes" : 13820182528,
            "available_in_bytes" : 12869599232,
            "spins" : "true"
          }
        ],
        "io_stats" : {
          "devices" : [
            {
              "device_name" : "sda2",
              "operations" : 496,
              "read_operations" : 37,
              "write_operations" : 459,
              "read_kilobytes" : 440,
              "write_kilobytes" : 3296
            }
          ],
          "total" : {
            "operations" : 496,
            "read_operations" : 37,
            "write_operations" : 459,
            "read_kilobytes" : 440,
            "write_kilobytes" : 3296
          }
        }
      }
    }
  }
}
```

## Os information
```
[root@cdh1 ~]# curl -X GET 'http://localhost:9200/_nodes/stats/os?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "timestamp" : 1495238958135,
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39:9300",
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "os" : {
        "timestamp" : 1495238958137,
        "cpu" : {
          "percent" : 0,
          "load_average" : {
            "1m" : 0.0,
            "5m" : 0.0,
            "15m" : 0.0
          }
        },
        "mem" : {
          "total_in_bytes" : 3011731456,
          "free_in_bytes" : 143056896,
          "used_in_bytes" : 2868674560,
          "free_percent" : 5,
          "used_percent" : 95
        },
        "swap" : {
          "total_in_bytes" : 2147479552,
          "free_in_bytes" : 2147479552,
          "used_in_bytes" : 0
        }
      }
    }
  }
}
```

## Process statistics
```
[root@cdh1 ~]# curl -X GET 'http://localhost:9200/_nodes/stats/process?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "timestamp" : 1495239025169,
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39:9300",
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "process" : {
        "timestamp" : 1495239025170,
        "open_file_descriptors" : 132,
        "max_file_descriptors" : 131072,
        "cpu" : {
          "percent" : 0,
          "total_in_millis" : 17010
        },
        "mem" : {
          "total_virtual_in_bytes" : 4884328448
        }
      }
    }
  }
}
```

## Indices statistics
```
[root@cdh1 ~]# curl -X GET 'http://localhost:9200/_nodes/stats/indices?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "timestamp" : 1495239103473,
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39:9300",
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "indices" : {
        "docs" : {
          "count" : 0,
          "deleted" : 0
        },
        "store" : {
          "size_in_bytes" : 0,
          "throttle_time_in_millis" : 0
        },
        "indexing" : {
          "index_total" : 0,
          "index_time_in_millis" : 0,
          "index_current" : 0,
          "index_failed" : 0,
          "delete_total" : 0,
          "delete_time_in_millis" : 0,
          "delete_current" : 0,
          "noop_update_total" : 0,
          "is_throttled" : false,
          "throttle_time_in_millis" : 0
        },
        "get" : {
          "total" : 0,
          "time_in_millis" : 0,
          "exists_total" : 0,
          "exists_time_in_millis" : 0,
          "missing_total" : 0,
          "missing_time_in_millis" : 0,
          "current" : 0
        },
        "search" : {
          "open_contexts" : 0,
          "query_total" : 0,
          "query_time_in_millis" : 0,
          "query_current" : 0,
          "fetch_total" : 0,
          "fetch_time_in_millis" : 0,
          "fetch_current" : 0,
          "scroll_total" : 0,
          "scroll_time_in_millis" : 0,
          "scroll_current" : 0,
          "suggest_total" : 0,
          "suggest_time_in_millis" : 0,
          "suggest_current" : 0
        },
        "merges" : {
          "current" : 0,
          "current_docs" : 0,
          "current_size_in_bytes" : 0,
          "total" : 0,
          "total_time_in_millis" : 0,
          "total_docs" : 0,
          "total_size_in_bytes" : 0,
          "total_stopped_time_in_millis" : 0,
          "total_throttled_time_in_millis" : 0,
          "total_auto_throttle_in_bytes" : 0
        },
        "refresh" : {
          "total" : 0,
          "total_time_in_millis" : 0,
          "listeners" : 0
        },
        "flush" : {
          "total" : 0,
          "total_time_in_millis" : 0
        },
        "warmer" : {
          "current" : 0,
          "total" : 0,
          "total_time_in_millis" : 0
        },
        "query_cache" : {
          "memory_size_in_bytes" : 0,
          "total_count" : 0,
          "hit_count" : 0,
          "miss_count" : 0,
          "cache_size" : 0,
          "cache_count" : 0,
          "evictions" : 0
        },
        "fielddata" : {
          "memory_size_in_bytes" : 0,
          "evictions" : 0
        },
        "completion" : {
          "size_in_bytes" : 0
        },
        "segments" : {
          "count" : 0,
          "memory_in_bytes" : 0,
          "terms_memory_in_bytes" : 0,
          "stored_fields_memory_in_bytes" : 0,
          "term_vectors_memory_in_bytes" : 0,
          "norms_memory_in_bytes" : 0,
          "points_memory_in_bytes" : 0,
          "doc_values_memory_in_bytes" : 0,
          "index_writer_memory_in_bytes" : 0,
          "version_map_memory_in_bytes" : 0,
          "fixed_bit_set_memory_in_bytes" : 0,
          "max_unsafe_auto_id_timestamp" : -9223372036854775808,
          "file_sizes" : { }
        },
        "translog" : {
          "operations" : 0,
          "size_in_bytes" : 0
        },
        "request_cache" : {
          "memory_size_in_bytes" : 0,
          "evictions" : 0,
          "hit_count" : 0,
          "miss_count" : 0
        },
        "recovery" : {
          "current_as_source" : 0,
          "current_as_target" : 0,
          "throttle_time_in_millis" : 0
        }
      }
    }
  }
}

```
Supported metrics are as follows:
* completion
* docs
* fielddata
* flush
* get
* indexing
* merge
* query_cache
* recovery
* refresh
* request_cache
* search
* segments
* store
* suggest
* translog
* warmer




