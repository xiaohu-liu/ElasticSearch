### Installation
* install the jdk version 8 and test with the command line `java  --version`
* download the binaries you want to install from http://www.elastic.co/downloads 

download the Elasticsearch 5.5.0 tar as example for the items below

* down load the target release version as follow
```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.5.0.tar.gz

```

* then extract it as follow
```
tar -xvf elasticsearch-5.5.0.tar.gz
```

* It will then create a bunch of files and folders in your current directory. We then go into the bin directory as follows and start the node and our single cluster as follow:
```
cd elasticsearch-5.5.0/bin && ./elasticsearch
```

<strong>Note:</strong> We can overwrite the cluster name and node name with custome as follow:
```
./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name
```

by default , the es provide access to it's api from port 9200 , and the port is configurable if neccesary.
   
