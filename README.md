Role Name
=========

Downloads all roles required to build a complete ELK-Stack.

Requirements
------------

Install
````
ansible-galaxy install mrlesmithjr.elkstack
````

You will need to define variables for each role that makes up this package after installing.
#####mrlesmithjr.elk-broker https://galaxy.ansible.com/list#/roles/4643
#####mrlesmithjr.elk-es https://galaxy.ansible.com/list#/roles/4644
#####mrlesmithjr.elk-haproxy https://galaxy.ansible.com/list#/roles/4663
#####mrlesmithjr.elk-kibana https://galaxy.ansible.com/list#/roles/4631
#####mrlesmithjr.elk-pre-processor https://galaxy.ansible.com/list#/roles/4671
#####mrlesmithjr.elk-processor https://galaxy.ansible.com/list#/roles/4676

Role Variables
--------------

Below are some examples to get you started.

group_vars/all/network
````
---
pri_domain_name: vagrant.local
````
group_vars/all/snmp
````
---
snmpd_authorized_networks:  #define read-only snmpd settings
  - network: 10.0.15.0/24
    community: vagrant
````
group_vars/all/syslog
````
---
configure_rsyslog: true
syslog_servers:
  - name: 10.0.15.100
    proto: tcp
    port: 514
````
group_vars/elk-broker-nodes
````
---
es_data_node: false
es_master_node: true
````
group_vars/elk-es-nodes
````
---
es_data_node: true
es_master_node: false
es_memory_tuning:  #these settings help eliminate OOM conditions (More memory should be used in most cases but these settings can help) #define here or in group_vars/group
  - name: indices.breaker.fielddata.limit
    set: false
    value: 60%  #default 60%
  - name: indices.breaker.request.limit
    set: false
    value: 40%  #default 40%
  - name: indices.breaker.total.limit
    set: false
    value: 40%  #default 40%
  - name: indices.fielddata.cache.size
    set: true
    value: 40%  #default undefined
````
group_vars/elk-haproxy-nodes
````
---
config_haproxy: true
config_keepalived: true
keepalived_router_id: 50
keepalived_vip: 10.0.15.100
keepalived_vip_int: eth1
````
group_vars/elk-nodes
````
---
config_hosts_file: true
es_cluster_name: vagrant
es_cluster_setup: true
es_network_publish_host: _eth1:ipv4_
es_min_master_nodes: 2
logstash_server_fqdn: 10.0.15.100
redis_allow_remote_connections: true
vagrant_deployment: true
````
group_vars/elk-processor-nodes
````
---
es_data_node: false
es_master_node: false
````

Dependencies
------------

#####mrlesmithjr.elk-broker https://galaxy.ansible.com/list#/roles/4643
#####mrlesmithjr.elk-es https://galaxy.ansible.com/list#/roles/4644
#####mrlesmithjr.elk-haproxy https://galaxy.ansible.com/list#/roles/4663
#####mrlesmithjr.elk-kibana https://galaxy.ansible.com/list#/roles/4631
#####mrlesmithjr.elk-pre-processor https://galaxy.ansible.com/list#/roles/4671
##### mrlesmithjr.elk-processor https://galaxy.ansible.com/list#/roles/4676

Example Playbook
----------------

inventory hosts file
````
[elk-nodes]
elk-broker-[1:3]
elk-es-[1:2]
elk-haproxy-[1:2]
elk-pre-processor-[1:2]
elk-processor-[1:2]

[elk-broker-nodes]
elk-broker-[1:3]

[elk-es-nodes]
elk-es-[1:2]

[elk-haproxy-nodes]
elk-haproxy-[1:2]

[elk-pre-processor-nodes]
elk-pre-processor-[1:2]

[elk-processor-nodes]
elk-processor-[1:2]
````
playbook.yml
````
---
- hosts: all
  sudo: true
  roles:
    - mrlesmithjr.bootstrap
    - mrlesmithjr.ntp
    - mrlesmithjr.rsyslog
    - mrlesmithjr.timezone

- hosts: elk-broker-nodes
  sudo: true
  roles:
    - mrlesmithjr.elk-broker

- hosts: elk-es-nodes
  sudo: true
  roles:
    - mrlesmithjr.elk-es

- hosts: elk-processor-nodes
  sudo: true
  roles:
    - mrlesmithjr.elk-processor

- hosts: elk-pre-processor-nodes
  sudo: true
  roles:
    - mrlesmithjr.elk-pre-processor

- hosts: elk-haproxy-nodes
  sudo: true
  roles:
    - mrlesmithjr.elk-haproxy
````

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
