# cluster reroute

The reroute command allows to explicitly execute a cluster reroute allocation command including specific commands. 

For example, a shard can be `moved from one node to another` explicitly, an `allocation can be canceled`, or an unassigned shard can be explicitly `allocated on a specific node`.


for example here:
```
POST /_cluster/reroute
{
    "commands" : [
        {
            "move" : {
                "index" : "test", "shard" : 0,
                "from_node" : "node1", "to_node" : "node2"
            }
        },
        {
          "allocate_replica" : {
                "index" : "test", "shard" : 1,
                "node" : "node3"
          }
        }
    ]
}
```





