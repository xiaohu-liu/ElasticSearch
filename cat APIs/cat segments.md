# cat segments

The segments command provides low level information about the segments in the shards of an index. It provides information similar to the _segments endpoint. For example:
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/segments/bank?v'
index shard prirep ip           segment generation docs.count docs.deleted    size size.memory committed searchable version compound
bank  0     p      10.100.0.146 _0               0        197            0 126.3kb       10511 true      true       6.5.0   true
bank  1     p      10.100.0.146 _0               0        191            0 122.6kb       10296 true      true       6.5.0   true
bank  2     p      10.100.0.146 _0               0         62            0  46.1kb        7824 true      true       6.5.0   true
bank  2     p      10.100.0.146 _1               1        149            0  96.5kb        9559 true      true       6.5.0   true
bank  3     p      10.100.0.146 _0               0        200            0 127.9kb       10542 true      true       6.5.0   true
bank  4     p      10.100.0.146 _0               0        153            0  98.9kb        9660 true      true       6.5.0   true
bank  4     p      10.100.0.146 _1               1         48            0  37.4kb        7581 true      true       6.5.0   true
```

If you only want to get information about segments in one particular index, you can add the index name in the URL, for example /_cat/segments/test. Also, several indexes can be queried like /_cat/segments/test,test1
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/segments/tw*?v'
index   shard prirep ip           segment generation docs.count docs.deleted  size size.memory committed searchable version compound
twitter 0     p      10.100.0.146 _1               1          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 0     p      10.100.0.146 _2               2          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 0     p      10.100.0.146 _3               3          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 0     p      10.100.0.146 _5               5          1            0 4.7kb        3134 true      true       6.5.0   true
twitter 0     p      10.100.0.146 _6               6          1            0 3.2kb        2042 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _0               0          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _1               1          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _2               2          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _3               3          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _5               5          1            0 4.2kb        2645 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _b              11          1            0 2.9kb        1497 true      true       6.5.0   true
twitter 1     p      10.100.0.146 _c              12          1            0 3.2kb        2042 true      true       6.5.0   true
twitter 2     p      10.100.0.146 _x              33         10            0 8.7kb        5402 true      true       6.5.0   false
twitter 3     p      10.100.0.146 _a              10          1            0 3.5kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _b              11          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _c              12          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _d              13          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _e              14          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _f              15          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _g              16          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _h              17          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 3     p      10.100.0.146 _i              18          1            0 2.2kb        1191 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _0               0          1            0 3.3kb        2042 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _1               1          1            0 3.5kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _2               2          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _3               3          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _4               4          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _5               5          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _6               6          1            0 3.6kb        2043 true      true       6.5.0   true
twitter 4     p      10.100.0.146 _7               7          1            0 4.4kb        2589 true      true       6.5.0   true
```
the detail meaning of the head listed as follows:
`prirep`
Whether this segment belongs to a primary or replica shard.
`ip`
The ip address of the segment’s shard.
`segment`
A segment name, derived from the segment generation. The name is internally used to generate the file names in the directory of the shard this segment belongs to.
`generation`
The generation number is incremented with each segment that is written. The name of the segment is derived from this generation number.
`docs.count`
The number of non-deleted documents that are stored in this segment. Note that these are Lucene documents, so the count will include hidden documents (e.g. from nested types).
`docs.deleted`
The number of deleted documents that are stored in this segment. It is perfectly fine if this number is greater than 0, space is going to be reclaimed when this segment gets merged.
`size`
The amount of disk space that this segment uses.
`size.memory`
Segments store some data into memory in order to be searchable efficiently. This column shows the number of bytes in memory that are used.
`committed`
Whether the segment has been sync’ed on disk. Segments that are committed would survive a hard reboot. No need to worry in case of false, the data from uncommitted segments is also stored in the transaction log so that Elasticsearch is able to replay changes on the next start.
`searchable`
True if the segment is searchable. A value of false would most likely mean that the segment has been written to disk but no refresh occurred since then to make it searchable.
`version`
The version of Lucene that has been used to write this segment.
`compound`
Whether the segment is stored in a compound file. When true, this means that Lucene merged all files from the segment in a single one in order to save file descriptors.
