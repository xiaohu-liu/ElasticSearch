# cluster update settting


Allows to update cluster wide specific settings. 

Settings updated can either be persistent (applied cross restarts) or transient (will not survive a full cluster restart). Here is an example:
```
[xiaohu-liu@cdh1 ~]$ curl -X PUT 'http://localhost:9200/_cluster/settings?pretty' -d '{ "persistent" : {"indices.recovery.max_bytes_per_sec" : "50mb"}}'
{
  "acknowledged" : true,
  "persistent" : {
    "indices" : {
      "recovery" : {
        "max_bytes_per_sec" : "50mb"
      }
    }
  },
  "transient" : { }
}
```

Resetting persistent or transient settings can be done by assigning a null value. Here is an example:
```
[xiaohu-liu@cdh1 ~]$ curl -X PUT 'http://localhost:9200/_cluster/settings?pretty' -d '{ "persistent" : {"indices.recovery.max_bytes_per_sec" : null}}'
{
  "acknowledged" : true,
  "persistent" : { },
  "transient" : { }
}
```

## Precedence of settings
Transient cluster settings > persistent cluster settings >  settings configured in the elasticsearch.yml config file. and the elasticsearch.yml is used for local configuration , but the settings api is just for the cluster-wider settings. 


