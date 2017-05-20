# nodes info

The cluster nodes info API allows to retrieve one or more (or all) of the cluster nodes information.
for example here:
```
curl -XGET 'http://localhost:9200/_nodes'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2'
```
```
[root@cdh1 ~]# curl -XGET 'http://localhost:9200/_nodes/cdh1?pretty'
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "xiaohu-liu",
  "nodes" : {
    "-foTUedtRtuy95CQ0UoLwA" : {
      "name" : "cdh1",
      "transport_address" : "192.168.1.39:9300",
      "host" : "192.168.1.39",
      "ip" : "192.168.1.39",
      "version" : "5.4.0",
      "build_hash" : "780f8c4",
      "total_indexing_buffer" : 213005107,
      "roles" : [
        "master",
        "data",
        "ingest"
      ],
      "settings" : {
        "pidfile" : "123456",
        "cluster" : {
          "name" : "xiaohu-liu"
        },
        "node" : {
          "name" : "cdh1"
        },
        "path" : {
          "logs" : "/opt/elasticsearch/elasticsearch-5.4.0/logs",
          "home" : "/opt/elasticsearch/elasticsearch-5.4.0"
        },
        "client" : {
          "type" : "node"
        },
        "http" : {
          "type" : {
            "default" : "netty4"
          }
        },
        "bootstrap" : {
          "memory_lock" : "false",
          "system_call_filter" : "false"
        },
        "transport" : {
          "type" : {
            "default" : "netty4"
          }
        },
        "network" : {
          "host" : "0.0.0.0"
        }
      },
      "os" : {
        "refresh_interval_in_millis" : 1000,
        "name" : "Linux",
        "arch" : "amd64",
        "version" : "2.6.32-573.el6.x86_64",
        "available_processors" : 2,
        "allocated_processors" : 2
      },
      "process" : {
        "refresh_interval_in_millis" : 1000,
        "id" : 3184,
        "mlockall" : false
      },
      "jvm" : {
        "pid" : 3184,
        "version" : "1.8.0_131",
        "vm_name" : "Java HotSpot(TM) 64-Bit Server VM",
        "vm_version" : "25.131-b11",
        "vm_vendor" : "Oracle Corporation",
        "start_time_in_millis" : 1495212937668,
        "mem" : {
          "heap_init_in_bytes" : 2147483648,
          "heap_max_in_bytes" : 2130051072,
          "non_heap_init_in_bytes" : 2555904,
          "non_heap_max_in_bytes" : 0,
          "direct_max_in_bytes" : 2130051072
        },
        "gc_collectors" : [
          "ParNew",
          "ConcurrentMarkSweep"
        ],
        "memory_pools" : [
          "Code Cache",
          "Metaspace",
          "Compressed Class Space",
          "Par Eden Space",
          "Par Survivor Space",
          "CMS Old Gen"
        ],
        "using_compressed_ordinary_object_pointers" : "true"
      },
      "thread_pool" : {
        "force_merge" : {
          "type" : "fixed",
          "min" : 1,
          "max" : 1,
          "queue_size" : -1
        },
        "fetch_shard_started" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 4,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "listener" : {
          "type" : "fixed",
          "min" : 1,
          "max" : 1,
          "queue_size" : -1
        },
        "index" : {
          "type" : "fixed",
          "min" : 2,
          "max" : 2,
          "queue_size" : 200
        },
        "refresh" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 1,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "generic" : {
          "type" : "scaling",
          "min" : 4,
          "max" : 128,
          "keep_alive" : "30s",
          "queue_size" : -1
        },
        "warmer" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 1,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "search" : {
          "type" : "fixed",
          "min" : 4,
          "max" : 4,
          "queue_size" : 1000
        },
        "flush" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 1,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "fetch_shard_store" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 4,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "management" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 5,
          "keep_alive" : "5m",
          "queue_size" : -1
        },
        "get" : {
          "type" : "fixed",
          "min" : 2,
          "max" : 2,
          "queue_size" : 1000
        },
        "bulk" : {
          "type" : "fixed",
          "min" : 2,
          "max" : 2,
          "queue_size" : 200
        },
        "snapshot" : {
          "type" : "scaling",
          "min" : 1,
          "max" : 1,
          "keep_alive" : "5m",
          "queue_size" : -1
        }
      },
      "transport" : {
        "bound_address" : [
          "[::]:9300"
        ],
        "publish_address" : "192.168.1.39:9300",
        "profiles" : { }
      },
      "http" : {
        "bound_address" : [
          "[::]:9200"
        ],
        "publish_address" : "192.168.1.39:9200",
        "max_content_length_in_bytes" : 104857600
      },
      "plugins" : [ ],
      "modules" : [
        {
          "name" : "aggs-matrix-stats",
          "version" : "5.4.0",
          "description" : "Adds aggregations whose input are a list of numeric fields and output includes a matrix.",
          "classname" : "org.elasticsearch.search.aggregations.matrix.MatrixAggregationPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "ingest-common",
          "version" : "5.4.0",
          "description" : "Module for ingest processors that do not require additional security permissions or have large dependencies and resources",
          "classname" : "org.elasticsearch.ingest.common.IngestCommonPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "lang-expression",
          "version" : "5.4.0",
          "description" : "Lucene expressions integration for Elasticsearch",
          "classname" : "org.elasticsearch.script.expression.ExpressionPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "lang-groovy",
          "version" : "5.4.0",
          "description" : "Groovy scripting integration for Elasticsearch",
          "classname" : "org.elasticsearch.script.groovy.GroovyPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "lang-mustache",
          "version" : "5.4.0",
          "description" : "Mustache scripting integration for Elasticsearch",
          "classname" : "org.elasticsearch.script.mustache.MustachePlugin",
          "has_native_controller" : false
        },
        {
          "name" : "lang-painless",
          "version" : "5.4.0",
          "description" : "An easy, safe and fast scripting language for Elasticsearch",
          "classname" : "org.elasticsearch.painless.PainlessPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "percolator",
          "version" : "5.4.0",
          "description" : "Percolator module adds capability to index queries and query these queries by specifying documents",
          "classname" : "org.elasticsearch.percolator.PercolatorPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "reindex",
          "version" : "5.4.0",
          "description" : "The Reindex module adds APIs to reindex from one index to another or update documents in place.",
          "classname" : "org.elasticsearch.index.reindex.ReindexPlugin",
          "has_native_controller" : false
        },
        {
          "name" : "transport-netty3",
          "version" : "5.4.0",
          "description" : "Netty 3 based transport implementation",
          "classname" : "org.elasticsearch.transport.Netty3Plugin",
          "has_native_controller" : false
        },
        {
          "name" : "transport-netty4",
          "version" : "5.4.0",
          "description" : "Netty 4 based transport implementation",
          "classname" : "org.elasticsearch.transport.Netty4Plugin",
          "has_native_controller" : false
        }
      ],
      "ingest" : {
        "processors" : [
          {
            "type" : "append"
          },
          {
            "type" : "convert"
          },
          {
            "type" : "date"
          },
          {
            "type" : "date_index_name"
          },
          {
            "type" : "dot_expander"
          },
          {
            "type" : "fail"
          },
          {
            "type" : "foreach"
          },
          {
            "type" : "grok"
          },
          {
            "type" : "gsub"
          },
          {
            "type" : "join"
          },
          {
            "type" : "json"
          },
          {
            "type" : "kv"
          },
          {
            "type" : "lowercase"
          },
          {
            "type" : "remove"
          },
          {
            "type" : "rename"
          },
          {
            "type" : "script"
          },
          {
            "type" : "set"
          },
          {
            "type" : "sort"
          },
          {
            "type" : "split"
          },
          {
            "type" : "trim"
          },
          {
            "type" : "uppercase"
          }
        ]
      }
    }
  }
}
```
and you can filter the output by specifying the metrics you want:
```
curl -XGET 'http://localhost:9200/_nodes/process'
curl -XGET 'http://localhost:9200/_nodes/_all/process'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/jvm,process'
# same as above
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/info/jvm,process'
curl -XGET 'http://localhost:9200/_nodes/nodeId1,nodeId2/_all
```
