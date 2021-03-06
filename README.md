# Unifiedbeat

Unifiedbeat reads records from [Unified2](http://manual.snort.org/node44.html) binary files generated by network intrusion detection software and indexes the records in [Elasticsearch](https://www.elastic.co/).
Unified2 files are created by [IDS/IPS software](https://en.wikipedia.org/wiki/Intrusion_prevention_system)
such as [Snort](https://www.snort.org/) and [Suricata](http://suricata-ids.org/).

***

> Note: only ```output unified2: ...``` is supported in ```snort.conf```

***

> In addition to using Kibana, a GoLang web app called [Pakquery](https://github.com/cleesmith/pakquery)
> is also available for searching within the data indexed by unifiedbeat. Pakquery's searches
> use the same simple Lucene syntax as in Kibana. However, pakquery is aware of the connection
> between **event and packet record types** based on the **event_id** field. This means that
> one can click on an event record and see the complete event/packet details, or one can
> click on a packet record and see the complete event/packet details.

***

#### Usage

1. build from source
1. ```curl -XPUT 'http://localhost:9200/_template/unifiedbeat' -d@etc/unifiedbeat.template.json```
1. edit ```unifiedbeat.yml```
1. **./unifiedbeat** -c unifiedbeat.yml

***
***
