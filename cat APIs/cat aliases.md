
# cat aliases
aliases shows information about currently configured aliases to indices including filter and routing infos.

```
[xiaohu-liu@cdh1 tmp]$ curl -X GET 'http://localhost:9200/_cat/aliases?v'
alias index filter routing.index routing.search
alias1 test1 -      -            -
alias2 test1 *      -            -
alias3 test1 -      1            1
alias4 test1 -      2            1,2
```
the output shows that `alias2` has configured a filter, and specific routing configuration in `alias3` and `alias4`

If you only want to get information about a single alias, you can specify the alias in the URL, for example /_cat/aliases/alias1.
