# Ansible role MongoDB
[![CI Molecule](https://github.com/darexsu/ansible-role-mongodb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mongodb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/58543?color=blue&label=downloads)


  - Role:
      - [platforms](#platforms)
      - [install](#install)
      - [merge behaviour](#merge-behaviour)
  - Playbooks (merge version):
      - [install and configure: MongoDB v5.0](#install-and-configure-mongodb-merge-version)
          - [install: MongoDB](#install-mongodb-merge-version)
          - [configure: MongoDB](#configure-mongodb-merge-version)
  - Playbooks (full version):
      - [install and configure: MongoDB v5.0](#install-and-configure-mongodb-full-version)
          - [install: MongoDB](#install-mongodb-full-version)
          - [configure: MongoDB](#configure-mongodb-full-version)

### Platforms

|  Testing         | repo: mongodb      |
| :--------------: | :----------------: |
| Debian 11        |  mongodb.com       |
| Debian 10        |  mongodb.com       |
| Ubuntu 20.04     |  mongodb.com       |
| Ubuntu 18.04     |  mongodb.com       |
| Oracle Linux 8   |  mongodb.com       |
| Rocky Linux 8    |  mongodb.com       |

### Install

```
ansible-galaxy install darexsu.mongodb --force
```

### Merge behaviour

Replace or Merge dictionaries (with "hash_behaviour=replace" in ansible.cfg):
```
# Replace             # Merge
---                   ---
  vars:                 vars:
    dict:                 merge:
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"

# How does merge work?:
Your vars [host_vars]  -->  default vars [current role] --> default vars [include role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```

##### Install and configure: MongoDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MongoDB
      mongodb:
        enabled: true
        version: "5.0"
      # MongoDB -> install
      mongodb_install:
        enabled: true
      # MongoDB -> config
      mongodb_config:
        enabled: true
        file: "mongod.conf"
        src: "mongod_conf_{{ ansible_os_family }}.j2"
        backup: true
        data: |
          systemLog:
            destination: file
            logAppend: true
            path: /var/log/mongodb/mongod.log
          storage:
            dbPath: /var/lib/mongo
            journal:
              enabled: true
          processManagement:
            fork: true  # fork and run in background
            pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
            timeZoneInfo: /usr/share/zoneinfo
          net:
            port: 27017
            bindIp: 127.0.0.1

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```
##### Install: MongoDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MongoDB
      mongodb:
        enabled: true
        version: "5.0"
      # MongoDB -> install
      mongodb_install:
        enabled: true

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```
##### Configure: MongoDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MongoDB
      mongodb:
        enabled: true
      # MongoDB -> config
      mongodb_config:
        enabled: true
        data: |
          systemLog:
            destination: file
            logAppend: true
            path: /var/log/mongodb/mongod.log
          storage:
            dbPath: /var/lib/mongo
            journal:
              enabled: true
          processManagement:
            fork: true  # fork and run in background
            pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
            timeZoneInfo: /usr/share/zoneinfo
          net:
            port: 27017
            bindIp: 127.0.0.1

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```
##### Install and configure: MongoDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # MongoDB
    mongodb:
      enabled: true
      version: "5.0"
      repo: "mongodb"
      service:
        enabled: true
        state: "started"
    # MongoDB -> install
    mongodb_install:
      enabled: true
    # MongoDB -> config
    mongodb_config:
      enabled: true
      file: "mongod.conf"
      src: "mongod_conf_{{ ansible_os_family }}.j2"
      backup: true
      data: |
        systemLog:
          destination: file
          logAppend: true
          path: /var/log/mongodb/mongod.log
        storage:
          dbPath: /var/lib/mongo
          journal:
            enabled: true
        processManagement:
          fork: true  # fork and run in background
          pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
          timeZoneInfo: /usr/share/zoneinfo
        net:
          port: 27017
          bindIp: 127.0.0.1

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```
##### Install: MongoDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # MongoDB
    mongodb:
      enabled: true
      version: "5.0"
      repo: "mongodb"
      service:
        enabled: true
        state: "started"
    # MongoDB -> install
    mongodb_install:
      enabled: true

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```
##### Configure: MongoDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # MongoDB
    mongodb:
      enabled: true
      version: "5.0"
      repo: "mongodb"
      service:
        enabled: true
        state: "started"
    # MongoDB -> config
    mongodb_config:
      enabled: true
      file: "mongod.conf"
      src: "mongod_conf_{{ ansible_os_family }}.j2"
      backup: true
      data: |
        systemLog:
          destination: file
          logAppend: true
          path: /var/log/mongodb/mongod.log
        storage:
          dbPath: /var/lib/mongo
          journal:
            enabled: true
        processManagement:
          fork: true  # fork and run in background
          pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
          timeZoneInfo: /usr/share/zoneinfo
        net:
          port: 27017
          bindIp: 127.0.0.1

  tasks:
    - name: role darexsu.mongodb
      include_role:
        name: darexsu.mongodb
```