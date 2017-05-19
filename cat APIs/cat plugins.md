# cat plugins
The plugins command provides a view per node of running plugins
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/plugins?v&s=component&h=name,component,version,description'
name component version description

```
