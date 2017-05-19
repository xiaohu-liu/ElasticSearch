# cluster state apihe
Cluster Stats API allows to retrieve statistics from a cluster wide perspective.
what about the information returned:
* basic index metrics (shard numbers, store size, memory usage) 
* the current nodes that form the cluster (number, roles, os, jvm versions, memory usage, cpu and installed plugins)

let's take an example to demonstrate:
```
[xiaohu-liu@cdh1 ~]$ curl -XGET 'http://localhost:9200/_cluster/stats?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "timestamp" : 1495194244112,
  "status" : "yellow",
  "indices" : {
    "count" : 5,
    "shards" : {
      "total" : 25,
      "primaries" : 25,
      "replication" : 0.0,
      "index" : {
        "shards" : {
          "min" : 5,
          "max" : 5,
          "avg" : 5.0
        },
        "primaries" : {
          "min" : 5,
          "max" : 5,
          "avg" : 5.0
        },
        "replication" : {
          "min" : 0.0,
          "max" : 0.0,
          "avg" : 0.0
        }
      }
    },
    "docs" : {
      "count" : 1041,
      "deleted" : 0
    },
    "store" : {
      "size_in_bytes" : 801035,
      "throttle_time_in_millis" : 0
    },
    "fielddata" : {
      "memory_size_in_bytes" : 0,
      "evictions" : 0
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
    "completion" : {
      "size_in_bytes" : 0
    },
    "segments" : {
      "count" : 39,
      "memory_in_bytes" : 135481,
      "terms_memory_in_bytes" : 88757,
      "stored_fields_memory_in_bytes" : 12152,
      "term_vectors_memory_in_bytes" : 0,
      "norms_memory_in_bytes" : 8512,
      "points_memory_in_bytes" : 64,
      "doc_values_memory_in_bytes" : 25996,
      "index_writer_memory_in_bytes" : 0,
      "version_map_memory_in_bytes" : 0,
      "fixed_bit_set_memory_in_bytes" : 0,
      "max_unsafe_auto_id_timestamp" : -1,
      "file_sizes" : { }
    }
  },
  "nodes" : {
    "count" : {
      "total" : 1,
      "data" : 1,
      "coordinating_only" : 0,
      "master" : 1,
      "ingest" : 1
    },
    "versions" : [
      "5.4.0"
    ],
    "os" : {
      "available_processors" : 2,
      "allocated_processors" : 2,
      "names" : [
        {
          "name" : "Linux",
          "count" : 1
        }
      ],
      "mem" : {
        "total_in_bytes" : 4002324480,
        "free_in_bytes" : 694820864,
        "used_in_bytes" : 3307503616,
        "free_percent" : 17,
        "used_percent" : 83
      }
    },
    "process" : {
      "cpu" : {
        "percent" : 0
      },
      "open_file_descriptors" : {
        "min" : 186,
        "max" : 186,
        "avg" : 186
      }
    },
    "jvm" : {
      "max_uptime_in_millis" : 91646574,
      "versions" : [
        {
          "version" : "1.8.0_131",
          "vm_name" : "Java HotSpot(TM) 64-Bit Server VM",
          "vm_version" : "25.131-b11",
          "vm_vendor" : "Oracle Corporation",
          "count" : 1
        }
      ],
      "mem" : {
        "heap_used_in_bytes" : 228855648,
        "heap_max_in_bytes" : 2130051072
      },
      "threads" : 43
    },
    "fs" : {
      "total_in_bytes" : 18579001344,
      "free_in_bytes" : 14605922304,
      "available_in_bytes" : 13655339008,
      "spins" : "true"
    },
    "plugins" : [ ],
    "network_types" : {
      "transport_types" : {
        "netty4" : 1
      },
      "http_types" : {
        "netty4" : 1
      }
    }
  }
}
```


