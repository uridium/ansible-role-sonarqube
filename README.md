Ansible role: SonarQube
=========
[![GitHub Actions](https://github.com/uridium/ansible-role-sonarqube/workflows/test-and-release/badge.svg)](https://github.com/uridium/ansible-role-sonarqube/actions)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-uridium.sonarqube-blue.svg)](https://galaxy.ansible.com/uridium/sonarqube)

An Ansible role that installs and configures SonarQube service on Debian stable.
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
---
sonarqube_version: '8.4.2.36762'
sonarqube_download_url: 'https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-{{ sonarqube_version }}.zip'

sonarqube_basedir: '/opt'
sonarqube_workdir: '{{ sonarqube_basedir }}/sonarqube'
sonarqube_lsb_script: '{{ sonarqube_workdir }}/bin/linux-x86-64/sonar.sh'

sonarqube_user: 'sonarqube'
sonarqube_group: 'sonarqube'

sonarqube_web_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'
sonarqube_ce_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'
sonarqube_search_java: '-Xmx2048m -Xms2048m -XX:+HeapDumpOnOutOfMemoryError'

sonarqube_vm_max_map_count: '262144'
sonarqube_fs_file_max: '65536'
sonarqube_nofile: '65536'
sonarqube_nproc: '4096'

db_host: YOURdbhost
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
---
- hosts: sonarqube.domain.io
  remote_user: boss
  become: True
  gather_facts: True

  roles:
    - role: uridium.sonarqube
      sonarqube_version: '8.9.8.54436'
      sonarqube_basedir: '/data'
      db_host: 'sonarqube_db.domain.io'
      db_name: 'sonarqube'
      db_user: 'sonarqube'
      db_pass: '{{ lookup("pass", "domain.io/db/sonarqube") }}'
```

License
-------

MIT
