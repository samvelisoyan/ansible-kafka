Kafka
=========

An Ansible Role that installs and configures Kafka and Zookeeper

Requirements
------------
### Operating Systems support

* Centos 7
* Redhat 7,8

Dependencies
------------

This role automatically can install JAVA (java-11-openjdk)
You(with good rights) can use 'users' role as a dependency, when it is required to create user accounts used by application (like 'kafka' user)

Variables and Properties
------------------------

In OPS ui, when you design your application, you have to use the software model(provider: ansible) related to this role
and so you benefited from data form

### Role Variables

| Variables | Required | Default value | Description |
|-----------|----------|---------------|-------------|
| kafka_version | False | *'2.8.0'* | Version of Kafka |
| kafka_scala_version | False | *'2.13'* | Version of Scala for Kafka |
| kafka_disable_install | False | *false* | Will skip packages installation, service manage and directories creation. |
| kafka_configure_zookeeper | False | *true* | Will install zookeeper with kafka, if disabled you need to have another zookeeper server and provide in kafka configuration |
| kafka_enabled_on_boot | False | *true* | Will start kafka on boot. |
| kafka_create_config | False | *true* | Will create custom config files. |
| kafka_disable_java_install | False | *false* | Will skip java packages installation |
| kafka_app_java_package | False | *'java-11-openjdk'* | Java package name which will be installed |
| kafka_max_open_files | False | *4098* | max number for open files, for systemd service |
| kafka_jmx_opts | False | *-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false* | pass the arguments for jmx |
| kafka_environment_vars | False | *[]* | List of environment vars for kafka |
| kafka_heap_opts | False | *"-Xmx256m -Xms256m"* | pass the arguments for jmx heap options |
| kafka_jmx_port | False | *9999* | port for jmx |
| kafka_broker_id | False | *0* | The broker id for this server.(https://kafka.apache.org/documentation/#brokerconfigs_broker.id) |
| kafka_broker_rack | False | *dc1* | Rack of the broker. This will be used in rack aware replication assignment for fault tolerance.(https://kafka.apache.org/documentation/#brokerconfigs_broker.rack) |
| kafka_log_dir | False | *"/var/log/kafka"* | Directory where logs will be stored |
| kafka_zookeeper_hosts | False | *- ip: localhost port: 2181* | list of zookeeper hosts which will be used by kafka |
| kafka_zookeeper_hosts:<br/>&nbsp;&nbsp;&nbsp;&nbsp;ip | False | ** | ip address of zookeeper host |
| kafka_zookeeper_hosts:<br/>&nbsp;&nbsp;&nbsp;&nbsp;port | False | ** | port number of zookeeper host |
| kafka_inter_broker_protocol_version | False | *2.1* | Specify which version of the inter-broker protocol will be used.(https://kafka.apache.org/documentation/#brokerconfigs_inter.broker.protocol.version) |
| kafka_zookeeper_data_dir | False | *"/var/lib/zookeeper"* | Directory where to stor zookeeper data |
| kafka_zookeeper_log_dir | False | *"/var/log/zookeeper"* | Directory where to stor zookeeper log |
### Role Vars file

By example, I would like to install(as root user) kafka and set configuration files, place following content into vars yaml file

```yaml
---
kafka_version: '2.8.0'
kafka_scala_version: '2.13'
kafka_disable_install: false
kafka_configure_zookeeper: true
kafka_enabled_on_boot: true
kafka_create_config: true
kafka_disable_java_install: false
kafka_app_java_package: 'java-11-openjdk'

kafka_max_open_files: 4098
kafka_jmx_opts: "-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false"
kafka_environment_vars: []
kafka_heap_opts: "-Xmx256m -Xms256m"
kafka_jmx_port: 9999

kafka_broker_id: 0
kafka_broker_rack: dc1
kafka_log_dir: "/var/log/kafka"
kafka_zookeeper_hosts:
  - ip: localhost
    port: 2181

kafka_inter_broker_protocol_version: 2.1
kafka_zookeeper_data_dir: "/var/lib/zookeeper"
kafka_zookeeper_log_dir: "/var/log/zookeeper"
```

Example Playbook
----------------

Including an example of how to use your role :
```yaml
---
    - hosts: servers
      vars_files:
        - vars_file.yml
      roles:
        - role: kafka
```

Tests
-----

The tests were done by using molecule integration test(inspec)
[More info](molecule-README.md#Requirements)

License
-------

Apache License

Author Information
------------------
samvelisoyan@hotmail.com
