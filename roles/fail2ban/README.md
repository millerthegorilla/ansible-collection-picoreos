Fail2ban
=========

Install, configure and start fail2ban systemd service

Requirements
------------

None particularly.

Role Variables
--------------

```
fail2ban_enabled: true
fail2ban_bantime: 10m
fail2ban_findtime: 10m
fail2ban_maxretry: 3
```
Dependencies
------------

None in particular

Example Playbook
----------------
```
- name: Fail2ban
  hosts: picoreos_hosts
  gather_facts: false
  become: true
  roles:
    - role: fail2ban
      tags: fail2ban
```
License
-------

Mit

Author Information
------------------

James Miller https://github.com/millerthegorilla
