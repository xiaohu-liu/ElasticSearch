# Identify the nodes in the cluster

how to identify a node in the cluster api?
* internal node id
* the node name
* address
* custom attributes
* the _local node receiving the request

for example:
```
# Local
GET /_nodes/_local
# Address
GET /_nodes/10.0.0.3,10.0.0.4
GET /_nodes/10.0.0.*
# Names
GET /_nodes/node_name_goes_here
GET /_nodes/node_name_goes_*
# Attributes (set something like node.attr.rack: 2 in the config)
GET /_nodes/rack:2
GET /_nodes/ra*:2
GET /_nodes/ra*:2*
```
