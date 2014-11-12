
Prerequisite
========

Check version of java for installing Elastic search. It is required that the version of Java be atleast 1.6
---------
$ java -version
java version "1.8.0"
Java(TM) SE Runtime Environment (build 1.8.0-b132)
Java HotSpot(TM) 64-Bit Server VM (build 25.0-b70, mixed mode)
----------

Versions of Software
========
Java 1.8
Elastic Search 1.3.1
Kibana 3.1.1
Fluent - td-agent 2.1.1
Fluent-plugin-search-0.6.1


Install Elasticsearch
=======
$brew install elasticsearch
-------
Start Elasticsearch
-------
ES_INCLUDE=/usr/local/etc/elasticsearch/bin/elasticsearch.in.sh elasticsearch -f

Install Kibana
=========
downlad https://download.elasticsearch.org/kibana/kibana/kibana-3.1.1.zip
---------
kibana-3.1.1]$python -m SimpleHTTPServer 8080
Serving HTTP on 0.0.0.0 port 8080 ...
————

Install Fluentd
=========

http://docs.fluentd.org/articles/install-by-dmg

Start Fluent
-------------
Start: launchctl load /Library/LaunchDaemons/td-agent.plistsudo 
Stop: launchctl unload /Library/LaunchDaemons/td-agent.plist

Configuration
------------
Edit /etc/td-agent/td-agent.conf and add the following to use logs created by application

-------------
<match opal_log>
  type elasticsearch
  type_name opal_log
  logstash_format true
</match>
<match opal_ctrl>
  type elasticsearch
  type_name opal_ctrl
  logstash_format true
 </match>
<source>
  type tail
  path ~/<MEDIVO_LOG>/development.log
  tag opal_log
  pos_file /var/log/td-agent/opal_log.pos
  format /(?<system>[a-z]+)\: (?<date>[^ ]* ?[^  ]*)(?<message>.*)$/
</source>
<source>
  type tail
  path ~/<MEDIVO_LOG>/control.txt
  tag opal_ctrl
  read_from_head true
  pos_file /var/log/td-agent/opal_ctrl.pos
  #format /(?<ctrl_type>[aA-zZ]+)\: (?<value>.*)$/
  format ltsv
  types numReaders:integer,numMappers:integer,bytesOut:integer,rejectedRecords:integer,batchStart:integer,numThreads:integer,numWriters:integer,bytesIn:integer,recordsInCount:integer,recordsOutCount:integer,batchStop:integer
</source>
-----------------

Install Elastic Search Plugin
===========================
sudo /opt/td-agent/embedded/bin/fluent-gem install fluent-plugin-elasticsearch


Verify
======

Any changes in the Log file will be automatically detected by the Fluent and log data will be loaded to Elastic search
