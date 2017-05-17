# description how to setup ElasticSearch and configure it
This section includes information on how to setup Elasticsearch and get it running, including:
* Downloading
* Installing
* Starting
* Configuring

## Supported Platforms
The matrix of officially supported operating systems and JVMs is available here: [Support Matrix. ](https://www.elastic.co/support/matrix)

### Java Version
* requires at least Java 8 in order to run, We recommend installing Java version 1.8.0_73 or later.
* only Oracle’s Java and the OpenJDK are supported
* setting the JAVA_HOME environment variable

<strong>Note: </strong>
* ships with default configuration for running Elasticsearch on `64-bit server JVMs`
* If you are using a 32-bit client JVM
   * you must remove -server from jvm.options
   * you should reconfigure the thread stack size from -Xss1m to -Xss320k 

