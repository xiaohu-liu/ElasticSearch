# cat templates

The templates command provides information about existing templates.
```
[xiaohu-liu@cdh1 ~]$ curl -X GET 'http://localhost:9200/_cat/templates?v'
name      template order version
template0 te*      0
template1 tea*     1
template2 teak*    2     7
```
The output shows that there are three existing templates, with template2 having a version value.
The endpoint also supports giving a template name or pattern in the url to filter the results, for example /_cat/templates/template* or /_cat/templates/template0.
