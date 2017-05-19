# cat fielddata
fielddata shows how much heap memory is currently being used by fielddata on every data node in the cluster.
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/fielddata?v'
id                     host         ip           node field   size
nwGPEZxBT5mAzA5lvS3SMQ 10.100.0.146 10.100.0.146 cdh1 _parent   0b
```

and you can specify the field you want to filter, like this:
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/fielddata?v&fields=_parent'
id                     host         ip           node field   size
nwGPEZxBT5mAzA5lvS3SMQ 10.100.0.146 10.100.0.146 cdh1 _parent   0b
```
And it accepts a comma delimited list:
```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/fielddata/_parent?v'
id                     host         ip           node field   size
nwGPEZxBT5mAzA5lvS3SMQ 10.100.0.146 10.100.0.146 cdh1 _parent   0b
```
