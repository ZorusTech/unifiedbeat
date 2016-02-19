# Unifiedbeat

Unifiedbeat reads records from [Unified2](http://manual.snort.org/node44.html) binary files generated by network intrusion detection software and indexes the records in [Elasticsearch](https://www.elastic.co/).
Unified2 files are created by [IDS/IPS software](https://en.wikipedia.org/wiki/Intrusion_prevention_system)
such as [Snort](https://www.snort.org/) and [Suricata](http://suricata-ids.org/).

> #### More
>
> * [Protect the Box](https://medium.com/@cleesmith/protect-the-box-c245acbaae81#.59j14oijl)
> * [From F to U and back again](https://medium.com/@cleesmith/from-f-to-u-and-back-again-9a021d643053#.l5qhab260)

> #### Note
>
> This version is no longer based on a clone of Filebeat, but follows the Beats development guide.
>
> See **CHANGELOG.md**

***

### February 18, 2016

#### Usage

1. build from source:
  * ```git clone https://github.com/cleesmith/unifiedbeat```
  * ```cd unifiedbeat```
  * ```go build```
    * if building for linux 64bit platform but on a mac do the following to cross-compile:
      * ```env GOOS=linux GOARCH=amd64 go build```
1. ```mkdir unifiedbeat```
1. ```cd unifiedbeat```
1. copy or scp the unifiedbeat binary file to the unifiedbeat folder
1. ```curl -XPUT 'http://localhost:9200/_template/unifiedbeat' -d@etc/unifiedbeat.template.json```
1. ```rm .unifiedbeat``` if exists ... this file tracks the previous positions within the unified2 files being tailed and indexed
1. ```vim etc/unifiedbeat.yml``` then change YAML configuration file:
  * **sensor**:
    * **unified2_path**: _?_  _... where are the unified2 files, typically: /var/log/snort/snort.log*_
    * **unified2_prefix**: "snort.log"
    * **rules**:
      * **gen_msg_map_path**: _?_  _... the absolute full path, typically: /etc/snort/gen-msg.map_
      * **paths**: _?_  _... where are the .rules files, typically: /etc/snort/rules/*.rules_
    * **fields**: _... add fixed/known details about this sensor_
  * . &nbsp; . &nbsp; .
  * **output**:
    * **elasticsearch**:
      * **hosts**: ["**?.?.?.?:9200**"]  _... elasticsearch's ip:port_
1. ```cp etc/unifiedbeat.yml /etc/unifiedbeat.yml``` ... this is not required but typically done
1. **./unifiedbeat** -c /etc/unifiedbeat.yml
  * typically this command would be in a systemd, Upstart, or SysV (init.d) script
  * for a quick test use:
    * ```nohup ./unifiedbeat -c /etc/unifiedbeat.yml &```
    * ```ps aux | grep -i unifiedbeat``` ... remember it's pid, so you can kill it
    * use curl, sense, or kibana to look at the indices in elasticsearch
    * ```kill ?pid?``` ... when done testing
1. now, use Kibana or a custom app daily to see what's up with your host and network, and sleep better at night

***

### Kibana screenshots

![Dashboard](https://raw.githubusercontent.com/cleesmith/unifiedbeat/master/screenshots/kibana_dashboard.png "example Kibana dashboard")

> this is just a simple example of a Kibana dashboard and not very useful for security analysts

> see kibana/export.json to import the provided dashboard, search, and visualizations into Kibana

> new to Kibana? this YouTube [playlist](https://www.youtube.com/playlist?list=PLhLSfisesZIvA8ad1J2DSdLWnTPtzWSfI) is helpful

***

#### Event record as shown in Kibana's Discover

![Event](https://raw.githubusercontent.com/cleesmith/unifiedbeat/master/screenshots/kibana_event_record.png "Kibana Discover event record")

> notice the **signature** and **rule_raw** fields

***

#### Packet record as shown in Kibana's Discover

![Packet](https://raw.githubusercontent.com/cleesmith/unifiedbeat/master/screenshots/kibana_packet_record.png "Kibana Discover packet record")

> notice the human readable **packet_dump** field with all layers shown in both hex and text

***

### Sense screenshots

#### Event type document in ElasticSearch

![Event](https://raw.githubusercontent.com/cleesmith/unifiedbeat/master/screenshots/unifiedbeat_event.png "typical Event type document in ElasticSearch")

***

#### Packet type document in ElasticSearch

![Packet](https://raw.githubusercontent.com/cleesmith/unifiedbeat/master/screenshots/unifiedbeat_packet.png "typical Packet type document in ElasticSearch")

***
***
