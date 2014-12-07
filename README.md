Role Name
=========

Ansible role to install and configure (multiple) Oracle Database(s) on RHEL based systems.

Requirements
------------

RHEL based Linux distribution.

Role Variables
--------------

The role does not have own variables yet. All used variables are listed in ansible-role-oracle-common (https://github.com/Nosmoht/ansible-role-oracle-common).

Dependencies
------------

Role ansible-role-oracle-common is required (https://github.com/Nosmoht/ansible-role-oracle-common).

Example Playbook
----------------

    - hosts: dbservers
      roles:
         - { role: Nosmoht.oracle-server }

License
-------

BSD

Author Information
------------------

