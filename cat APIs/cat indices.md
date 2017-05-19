# cat indices
The indices command provides a cross-section of each index, for example
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/indices/twi*,bank?v&s=index'
health status index    uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank     csBO4S-pThGhCRPB35Iy6g   5   1       1000            0    657.1kb        657.1kb
yellow open   twitter  lHkiUFEmTrKYugulAenzgA   5   1         39            0    116.1kb        116.1kb
yellow open   twitter2 E9JW2pWnSoyhqPr5V01Erw   5   1          0            0       679b           679b
```
We can tell quickly how many shards make up an index, the number of docs, deleted docs, primary store size, and total store size (all shards including replicas). All these exposed metrics come directly from Lucene APIs.

# Example
* Which indices are yellow?
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/indices/*?v&health=yellow'
health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank       csBO4S-pThGhCRPB35Iy6g   5   1       1000            0    657.1kb        657.1kb
yellow open   blogs      KFmiQkJNQ5apKw5aoBjvEA   5   1          1            0      4.2kb          4.2kb
yellow open   twitter2   E9JW2pWnSoyhqPr5V01Erw   5   1          0            0       679b           679b
yellow open   twitter    lHkiUFEmTrKYugulAenzgA   5   1         39            0    116.1kb        116.1kb
yellow open   xiaohu-liu mlquVn1tQaWdJ93tTsKyrg   5   1          1            0        4kb            4kb
```

* Which index has the largest number of documents?
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/indices/*,bank?v&s=docs.count:desc'
health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank       csBO4S-pThGhCRPB35Iy6g   5   1       1000            0    657.1kb        657.1kb
yellow open   twitter    lHkiUFEmTrKYugulAenzgA   5   1         39            0    116.1kb        116.1kb
yellow open   blogs      KFmiQkJNQ5apKw5aoBjvEA   5   1          1            0      4.2kb          4.2kb
yellow open   xiaohu-liu mlquVn1tQaWdJ93tTsKyrg   5   1          1            0        4kb            4kb
yellow open   twitter2   E9JW2pWnSoyhqPr5V01Erw   5   1          0            0       679b           679b
```

* How many merge operations have the shards for the twitter completed?
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/indices/twitter?pri&v&h=health,index,pri,rep,docs.count,mt'
health index   pri rep docs.count mt pri.mt
yellow twitter   5   1         39  1      1
```

* How much memory is used per index?
```
[xiaohu-liu@cdh1 pssh-2.3.1]$ curl -X GET 'http://localhost:9200/_cat/indices?v&h=i,tm&s=tm:desc'
i              tm
bank       64.4kb
twitter    63.9kb
blogs       2.2kb
xiaohu-liu  1.6kb
twitter2       0b
```
