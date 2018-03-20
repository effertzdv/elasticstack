# my elasticstack #

This a docker-compose collection to run an ELK stack with
 
- Logstash
- Elasticsearch
- Kibana
- curator 
- ElasticHQ

Logstash, ES and kibana are the default components. But while running some tests with fast growing syslog files I added curator to keep the number of indices limited and preserve diskspace.

Addtionally I added ElasticHQ - a great tool to manage your ElasticSearch installation.


## installation if you are behind a proxx ##

build the docker images if you have no direct connection to the internet will fail if you do not define the http_prpxy / https_proxy in your Dockerfile:

    ENV http_proxy=http://proxyuser:proxypass@proxy.ip:port
    ENV https_proxy=http://proxyuser:proxypass@proxy.ip:port
     
## Logstash configuration ##

The logstash pipeline has some preconfiured inputs und filters:

- syslog from unifi UAP accesspoints (udp/5142)
- filebeat from unifi controller log (tcp/5045)
- syslog from sonicwall TZ und NSA firewalls (udp 5140)
- filebeat from squid proxy access.log (tcp/5044)
- syslog from vmware vsphere hosts (udp/514)
 
The portnumbers refer to to public ports via the docker-compose file. The ports inside the logstash docker image might be different.

## first run ##

Install docker & docker-compose
clone the git repositoy
create the docker images with docker-compose up
wait for the initalization of the stack to complete

Then open kibana at http://my.host:5601 and create the indices. Before each index can be created make sure
you have pushed content from logstash to your indices ( simple let you log source push data to logstash - if the data arrive elasticsearch creates to indices for you). If the indices exist you can add them to kibana.

## Visualisations ##

... tbd
