oracle-server
==========
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Variables](#variables)
- [Usage](#usage)

# Introduction
Ansible role to install and configure (multiple) Oracle Database(s) running in different ORACLE_HOME's on RHEL based systems.

Features
- Prepare system to install Oracle Database or Oracle Grid Infrastructure
- Install required OS packages
- Set Kernel parameters (sysctl, limits)
- Create SysV or Systemd service file
- Install Database Software using runInstaller
- Install Database using dbca
- Setup Oracle Listener

Library
----------
- oracle_listener to handle Oracle Listeners (start/stop/status)

Not implement yet
- Grid Infrastructure installation and configuration
- Patching

# Requirements
- Ansible 1.9
- Ansible roles
  - [avahi](https://github.com/Nosmoht/ansible-role-avahi.git)
  - [kernel](https://github.com/Nosmoht/ansible-role-kernel.git)
  - [oracle-common](https://github.com/Nosmoht/ansible-role-oracle-common.git)
  - [pam-limits](https://github.com/Nosmoht/ansible-role-kernel.git)
- RHEL based Linux distribution.

# Variables

Most of the variables are listed in role [oracle-common].

# Usage
Following example will:
- Install Oracle 12.1.0.2 Enterprise Edition in /app/oracle/product/12.1.0/dbhome\_1
- Install Oracle 11.2.0.3 Standard Edition in /app/oracle/product/11.2.0/dbhome\_1.
- Create 12c Database
- Create 11g Database
- Setup a Listener using the 12c ORACLE_HOME

**Installation files must be on the remote system in \_installation\_files\_directory\_.**
```yaml
- hosts: dbservers
  vars:
    oracle_app_directory: /app
    oracle_db_home_1:
      name: dbhome_1
      version: 12.1.0.2
      edition: EE
      path: '{{ oracle_app_directory }}/oracle/product/12.1.0/dbhome_1'
      installation_files_directory: /share/oracle/12.1.0.2/patches
      unpack_directory: /share/oracle/12.1.0.2/install
      response_file: /tmp/dbhome_1.rsp
    oracle_db_home_2:
      name: dbhome
      version: 11.2.0.3
      edition: SE
      path: '{{ oracle_app_directory }}/oracle/product/11.2.0/dbhome_1'
      installation_files_directory: /share/oracle/11.2.0/patches
      unpack_directory: /share/oracle/11.2.0/install
      response_file: /tmp/dbhome_2.rsp
      oracle_listeners:
      - name: LISTENER
        protocol: TCP
        port: 1521
        oracle_home: dbhome_1
  role:
  - role: oracle-server
    oracle_db_home: '{{ oracle_db_home_1 }}'
    oracle_databases:
    - name: ORA12C
      oracle_home: dbhome_1
      install: true
      syspassword: ora12c
      systempassword: ora12c
      dbsnmppassword: ora12c
      characterset: AL32UTF8
      dbca_template_file: /tmp/ora12c.dbc
      common_attributes: []
  - role: oracle-server
    oracle_db_home: '{{ oracle_db_home_1 }}'
    oracle_databases:
    - name: ORA11G
      oracle_home: dbhome_2
      install: true
      syspassword: ora11gr2
      systempassword: ora11gr2
      dbsnmppassword: ora11gr2
      characterset: AL32UTF8
      dbca_template_file: /tmp/ora11g.dbc
      common_attributes: []
    - name: CLONEDB
      oracle_home: db_home2
      install: false
      common_attributes: []
```

# Author

[Thomas Krahn](mailto:ntbc@gmx.net)

[kernel]: https://github.com/Nosmoht/ansible-role-kernel
[oracle-common]: https://github.com/Nosmoht/ansible-role-oracle-common
