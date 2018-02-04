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

Additional variables, which defined in corresponding roles can be overwritten by adding variables with the same
name in playbook main.yml vars section. Example:

```yaml
vars:
  NGINX_DEFAULT_PASSWD: another_password # This variable will overwrite default variable `NGINX_DEFAULT_PASSWD` in
                                         # role `winmasta.nginx`
```

Dependencies
------------

Depends on:
 - winmasta.docker-latest
 - winmasta.nginx

 Example Playbook
 ----------------

 To use this role:

   - create folder (in user $HOME folder in example below) and install role with dependencies from ansible-galaxy

 ```bash
 cd ~/
 mkdir prometheus
 cd prometheus
 ansible-galaxy install winmasta.grafana --roles-path .
 ```

   - as soon as ansible-galaxy doesn't install role dependencies yet, you should do it manually

 ```bash
 ansible-galaxy install -r winmasta.grafana/requirements.yml --roles-path .
 ```

   - create file `hosts`, containing hostname(s) or IP address(es) of host(s), where you want to deploy prometheus

 ```bash
 echo "ENTER HOSTNAME OR IP" > hosts
 ```

   - create file `ansible.cfg` in current folder

 ```bash
 cat > ansible.cfg << EOF
 [defaults]
 remote_user = root
 host_key_checking = False
 EOF
 ```

   - create playbook in current folder `main.yml` with content

 ```bash
 cat > main.yml << EOF
 ---
 - hosts: all
   gather_facts: no

   pre_tasks:

   - name: Install required packages
     raw: sudo apt-get update -y && sudo apt-get -y install python-simplejson python-pip
     changed_when: False

   - setup:

   roles:
     - winmasta.docker-latest
     - winmasta.nginx
     - winmasta.prometheus
 EOF
 ```

   - execute playbook `main.yml`

 ```bash
 ansible-playbook -i hosts main.yml
 ```

License
-------

MIT
