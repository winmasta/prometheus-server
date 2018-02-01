Role Name
=========

Ansible role for latest prometheus server in docker container. UI behind nginx with basic authorization. Tested on:
  - Ubuntu 14.04 Trusty
  - Ubuntu 16.04 Xenial

Requirements
------------

Installed OS:
 - Ubuntu 14.04 Trusty
 - Ubuntu 16.04 Xenial

Role Variables
--------------

PROM_PORT: 9090 # Default prometheus port
SCRAPE_INTERVAL: 15s # Default prometheus metrics scrape interval

**Following variables can be changed after deployment by editing `/etc/prometheus/prometheus.yml` if you leave it
default. Don't forget to restart prometheus container `docker restart prometheus`**

CONSUL_JOB_NAME: consul # Prometheus job name for consul cluster
CONSUL_SERVICE_TAG: consul # Consul tag to include to consul job
CONSUL_DROP_TAG: consul-server # Consul tag to exclude consul servers
STATIC_JOB_NAME: hydra # Static job name
STATIC_JOB_TARGET: www.leningrad.spb.ru # Static job target DNS name or IP addsess
STATIC_JOB_PORT: 9100 # Static job port
CONFIG_DIR: /etc/prometheus # Prometheus config directory
CONFIG_FILENAME: prometheus.yml # Prometheus config filename

Dependencies
------------

Depends on:
 - winmasta.docker-latest
 - winmasta.nginx

License
-------

MIT
