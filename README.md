Ansible role: SonarQube
=========
[![Build Status](https://travis-ci.org/uridium/ansible-role-sonarqube.svg)](https://travis-ci.org/uridium/ansible-role-sonarqube)
[![Galaxy](https://img.shields.io/badge/galaxy-uridium.sonarqube-blue.svg)](https://galaxy.ansible.com/uridium/sonarqube)

An Ansible role that installs and configures SonarQube service on Debian/Ubuntu systems.
It uses PostgreSQL database to store configuration and snapshots.

Requirements
------------

* openjdk-11
* unzip
* postgresql server

`openjdk-11-jre-headless` and `unzip` packages are defined in `tasks/main.yml`.
PostgreSQL server needs to be installed beforhand, using other role.

[SonarQube requirements](https://docs.sonarqube.org/latest/requirements/requirements/)

Role Variables
--------------

Available variables are listed down below (see `defaults/main.yml`):

```yaml
    sonarqube_version: '8.2.0.32929'
    sonarqube_download_url: 'https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-{{ sonarqube_version }}.zip'

    sonarqube_workdir: '/opt/sonarqube'
    sonarqube_lsb_script: '{{ sonarqube_workdir }}/bin/linux-x86-64/sonar.sh'

    sonarqube_user: 'sonarqube'
    sonarqube_group: 'sonarqube'

    sonarqube_web_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'
    sonarqube_ce_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'
    sonarqube_search_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'

    max_map_count: '262144'
    nofile: '65536'
    nproc: '4096'

    db_host: YOURsonarqube.host
    db_name: YOURdbname
    db_user: YOURdbusername
    db_pass: YOURdbpassword
```

How to store encrypted passwords:

* using [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html):

```yaml
    db_pass: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          62383534356266343334383935326331386332356338663837373032643562653537373238373830
          6339353037386132663139393661333738303439316339650a393162373132626330633464353234
          66616137323661306666376666623330626535303436313931653962386361353537323833343863
          3862386566613462390a663362393236313765323036636439653763623933303334333533653234
          3033
```

* using [passwordstore plugin](https://docs.ansible.com/ansible/latest/plugins/lookup/passwordstore.html):

```yaml
    db_pass: '{{ lookup("pass", "path/to/your/passwordstore/file") }}'
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
    - hosts: sonarqube.host
      remote_user: admin
      become: True
      gather_facts: True

      roles:
        - role: uridium.sonarqube
          sonarqube_basedir: '/data'
```

License
-------

MIT
