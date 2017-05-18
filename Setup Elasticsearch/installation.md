## Installing ElasticSearch
There are several method to install elasticsearch on our machine.
* [Install Elasticsearch with .zip or .tar.gz](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html)
* [Install Elasticsearch with Debian Package](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)
* [Install Elasticsearch with RPM](https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html)
* [Install Elasticsearch on Windows](https://www.elastic.co/guide/en/elasticsearch/reference/current/windows.html)
* [Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)


### Install Elasticsearch with .zip or .tar.gz
pre-install
* you must install the jdk refer to the jdk requirement on your cluster

install
* download and install .zip package
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.0.zip
sha1sum elasticsearch-5.4.0.zip 
unzip elasticsearch-5.4.0.zip
cd elasticsearch-5.4.0/ 
```

* download and install .tar.gz package
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.0.tar.gz
sha1sum elasticsearch-5.4.0.tar.gz 
tar -xzf elasticsearch-5.4.0.tar.gz
cd elasticsearch-5.4.0/ 
```

run elasticsearch from the command line
* the command you should submit :
```
./bin/elasticsearch
```
By default, Elasticsearch runs in the foreground, prints its logs to the standard output (stdout), and can be stopped by pressing Ctrl-C.

run elasticsearch as a deamon
* To run Elasticsearch as a daemon, specify -d on the command line, and record the process ID in a file using the -p option:
```
./bin/elasticsearch -d -p pid
```
Log messages can be found in the $ES_HOME/logs/ directory.

To shut down Elasticsearch, kill the process ID recorded in the pid file:
```
kill `cat pid`
```

check the es is running
* you can access the uri `http://localhost:9200/` or use the `curl` command to access the uri as follow to check the status of es
```
GET /
```
response text:
```
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "5.4.0",
    "build_hash" : "f27399d",
    "build_date" : "2016-03-30T09:51:41.449Z",
    "build_snapshot" : false,
    "lucene_version" : "6.5.0"
  },
  "tagline" : "You Know, for Search"
}
```
Log printing to stdout can be disabled using the -q or --quiet option on the command line.


### Configure Elasticasearch
Elasticsearch loads its configuration from the $ES_HOME/config/elasticsearch.yml file by default

Any settings that can be specified in the config file can also be specified on the command line, using the -E syntax as follows:
```
./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1
```
<strong>Note: </strong> Typically, any cluster-wide settings (like cluster.name) should be added to the elasticsearch.yml config file, while any node-specific settings such as node.name could be specified on the command line.

### Directory Layout of ES
[Directory Layout](https://github.com/xiaohu-liu/ElasticSearch/edit/master/Setup%20Elasticsearch/es_directory_layout.png)



## Install with RPM
### Install Online
import the es PGP Key
* Download and install the public signing key:
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

install the es yum repository
* Create a file called elasticsearch.repo in the /etc/yum.repos.d/ directory for RedHat based distributions
```
[elasticsearch-5.x]
name=Elasticsearch repository for 5.x packages
baseurl=https://artifacts.elastic.co/packages/5.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```
* install Elasticsearch with the following commands:
 ```
sudo yum install elasticsearch 
 ```
### Install offline
the RPM for Elasticsearch v5.4.0 can be downloaded from the website and installed as follows:
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.0.rpm
sha1sum elasticsearch-5.4.0.rpm 
sudo rpm --install elasticsearch-5.4.0.rpm
```

### the configuration item with RPM
* location of configuration file: `/etc/elasticsearch/elasticsearch.yml`
* The RPM also has a system configuration file (/etc/sysconfig/elasticsearch), which allows you to set the following parameters:
```
ES_USER

      The user to run as, defaults to elasticsearch.

ES_GROUP

      The group to run as, defaults to elasticsearch.

JAVA_HOME

      Set a custom Java path to be used.

MAX_OPEN_FILES

      Maximum number of open files, defaults to 65536.

MAX_LOCKED_MEMORY

      Maximum locked memory size. Set to unlimited if you use the bootstrap.memory_lock option in elasticsearch.yml.

MAX_MAP_COUNT

      Maximum number of memory map areas a process may have. If you use mmapfs as index store type, make sure this is set to a high value. For more information, check the linux kernel documentation about max_map_count. This is set via sysctl before starting elasticsearch. Defaults to 262144.

LOG_DIR

    Log directory, defaults to /var/log/elasticsearch.

DATA_DIR

     Data directory, defaults to /var/lib/elasticsearch.

CONF_DIR

    Configuration file directory (which needs to include elasticsearch.yml and log4j2.properties files), defaults to /etc/elasticsearch.

ES_JAVA_OPTS

    Any additional JVM system properties you may want to apply.

RESTART_ON_UPGRADE

    Configure restart on package upgrade, defaults to false. This means you will have to restart your elasticsearch instance after installing a package manually. The reason for this is to ensure, that upgrades in a cluster do not result in a continuous shard reallocation resulting in high network traffic and reducing the response times of your cluster.
```
 
