# cat cluster
master doesn’t have any extra options. It simply displays the master’s node ID, bound IP address, and node name

for example as follows:
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/master?v'
id                     host         ip           node
nwGPEZxBT5mAzA5lvS3SMQ 10.100.0.146 10.100.0.146 cdh1
```

check the master of all nodes of the cluster
```
% pssh -i -h list.of.cluster.hosts curl -s localhost:9200/_cat/master
[1] 19:16:37 [SUCCESS] es3.vm
Ntgn2DcuTjGuXlhKDUD4vA 192.168.56.30 H5dfFeA
[2] 19:16:37 [SUCCESS] es2.vm
Ntgn2DcuTjGuXlhKDUD4vA 192.168.56.30 H5dfFeA
[3] 19:16:37 [SUCCESS] es1.vm
Ntgn2DcuTjGuXlhKDUD4vA 192.168.56.30 H5dfFeA
```
