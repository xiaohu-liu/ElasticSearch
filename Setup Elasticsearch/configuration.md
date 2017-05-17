## Configuration
* ships with good default and requires little configuration, most setting can be update dynamically on running cluster 
* configuration must contains settings which are node-specific (such as `node.name` and `paths`), or settings which a node requires in order to be able to join a cluster, such as `cluster.name` and `network.host`


### Config File Location
2 config files:
* `elasticsearch.yaml` for configuring elasticsearch, who's default location is `$ES_HOME/config`
* `log4j2.properties` for configuring es logging, who's default location is `$ES_HOME/config`
* if you install with RPM, the location for the 2 files is just `/etc/elasticsearch`
* The location of the config directory can be changed with the path.conf setting, as follows:
```
./bin/elasticsearch -Epath.conf=/path/to/my/config/
```
so you can dynamically specify the location when to start the es


### Config File Format
* Yaml, just as the example as follows:
```
path:
    data: /var/lib/elasticsearch
    logs: /var/log/elasticsearch
```
Settings can also be flattened as follows:
```
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```

### Environment variable subsitution
```
node.name:    ${HOSTNAME}
network.host: ${ES_NETWORK_HOST}
```
### Prompting for setting
```
node:
  name: ${prompt.text}
```
when to start the es with foreground, you will be prompted to enter the actual value like so:
```
Enter value for [node.name]:
```

## Important Configuration
```
* path.data and path.logs
* cluster.name
* node.name
* bootstrap.memory_lock
* network.host
* discovery.zen.ping.unicast.hosts
* discovery.zen.minimum_master_nodes
```

### path.data and path.logs
```
path:
  logs: /var/log/elasticsearch
  data: /var/data/elasticsearch
```
or 
```
path:
  data:
    - /mnt/elasticsearch_1
    - /mnt/elasticsearch_2
    - /mnt/elasticsearch_3
```

### cluster.name
A node can only join a cluster when it shares its cluster.name with all the other nodes in the cluster

### node.name
* By default, Elasticsearch will take the 7 first character of the randomly generated uuid used as the node id
* Note that the node id is persisted and does not change
* The node.name can also be set to the server’s HOSTNAME as follows:
```
node.name: ${HOSTNAME}
```
### bootstrap.memory_lock
* false

### network.host
* hostname

### discovery.zen.ping.unicast.hosts
* the hostname list of the node in the cluster

### discovery.zen.minimum_master_nodes
* (master_eligible_nodes / 2) + 1
