# cat snapshots

The snapshots command shows all snapshots that belong to a specific repository. To find a list of available repositories to query, the command /_cat/repositories can be used. Querying the snapshots of a repository named repo1 then looks as follows.

```
% curl 'localhost:9200/_cat/snapshots/repo1?v'
id     status start_epoch start_time end_epoch  end_time duration indices successful_shards failed_shards total_shards
snap1  FAILED 1445616705  18:11:45   1445616978 18:16:18     4.6m       1                 4             1            5
snap2 SUCCESS 1445634298  23:04:58   1445634672 23:11:12     6.2m       2                10             0           10

```

Each snapshot contains information about when it was started and stopped. Start and stop timestamps are available in two formats. The HH:MM:SS output is simply for quick human consumption. The epoch time retains more information, including date, and is machine sortable if the snapshot process spans days.
