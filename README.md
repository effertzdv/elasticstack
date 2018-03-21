# My Docker ELK Stack

This a docker-compose collection to run an ELK stack with
 
- Logstash
- Elasticsearch
- Kibana
- curator 
- ElasticHQ (see installation instruction)

Logstash, ES and kibana are the default components. But while running some tests with fast growing syslog files I added curator to keep the number of indices limited and preserve diskspace.

Addtionally I added ElasticHQ - a great tool to manage your ElasticSearch installation.

Most of this repository is based on the excellent work of Anthony Lapenna: [https://github.com/deviantony/docker-elk](https://github.com/deviantony/docker-elk). He made a very installation and configuration tutorial. Please visit his repo

I did only some tweaks here and there for my personal needs.

## Install Elasticsearch-HQ ##

clone the repository

    git clone https://github.com/ElasticHQ/elasticsearch-HQ.git

to the folder elasticsearch-HQ.

## installation if you are behind a proxy ##

build the docker images if you have no direct connection to the internet will fail if you do not define the http_prpxy / https_proxy in your Dockerfile:

    ENV http_proxy=http://proxyuser:proxypass@proxy.ip:port
    ENV https_proxy=http://proxyuser:proxypass@proxy.ip:port

     
## Logstash configuration ##

The logstash pipeline has some preconfiured inputs und filters:

- syslog from unifi UAP accesspoints (udp/5142)
- filebeat from unifi controller log (tcp/5560)
- syslog from sonicwall TZ und NSA firewalls (udp 5140)
- filebeat from squid proxy access.log (tcp/5044)
- syslog from vmware vsphere hosts (udp/514)
 
The portnumbers refer to to public ports via the docker-compose file. The ports inside the logstash docker image might be different.

## curator setup ##

see actions.yml for details:

Five jobs to keep indices for 14 days
- unifi-*
- unifiserver-*
- vmware-*
- squid-*
- sonicwall-*

Add more jobs for new indices. Of course curator can do more then delete the indices.
