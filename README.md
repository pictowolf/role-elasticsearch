Elasticsearch
=========

An Elasticsearch role that can deploy a standalone node or a cluster. All tested with molecule. Ansible role will deploy wither Elastic 7.x or Elastic 8.x

Requirements
------------

No requirements for this role.

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

```yaml
es_major_version Default value: 7
es_minor_version: 16.1-1
```
Setting the major and minor version of Elasticsearch you are wanting to install. This will also ensure the correct yum repository is added to your instances. Change the version and set es_update: true to upgrade the cluster. See more information below.

```yaml
es_upgrade: false
```
Runs upgrade tasks and will upgrade the cluster to the specified version. <strong>Ensure you have Serial: 1 set in your playbook for upgrading</strong>. Within the upgrade, shard allocation will be limited to primary shards only to ensure a quick upgrade. This follows Elasticsearch best pracise on rolling upgrades.

```yaml
es_cluster_name: dev-cluster
es_data_path: /data/elasticsearch
es_log_path: /var/log/elasticsearch
es_user: elasticsearch
es_group: elasticsearch
```
Basic cluster information including the path for logd and data.

```yaml
es_heap_size: 1g
```
Sets the JVM heap size to the value. This will set both xms and xmx to the same value. This should be half of your memory on the instance. Don't set this above 32GB or you will see performance issue, instead add more instances.

```yaml
es_start_service: true
es_restart_on_change: true
```
Both of these settings are if you don't want the service to start on install. EG, you have extra plugins to configure first. The restart_on_change will restart any time you ammend the elasticsearch.yml.

```yaml
es_bootstrap_password: testing
```
Sets the default password for the elastic user. This is required to login via any of the endpoints.

```yaml
es_tls_enabled: true
es_generate_certs: false
```
This will generate and setup transport TLS and HTTP TLS using self signed certificates from Elastic.

```yaml
es_tls_cert: /etc/elasticsearch/mycert.pem
es_tls_key: /etc/elasticsearch/mykey.pem
es_tls_ca: /etc/elasticsearch/myca.pem
```
Set these to use custom certificates that are not self signed. Ensure you set es_tls_enabled: true and to es_generate_certs: false.

Example Playbook
----------------
```yaml
---
- hosts: elastic
  roles:
      - pictowolf.elasticsearch
```
