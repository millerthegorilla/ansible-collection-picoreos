Server Hardening
================

Runs devsec hardening roles os_hardening and ssh_hardening from https://github.com/millerthegorilla/ansible-collection-hardening/ a fork that is customised to add hardening to immutable root systems

Will eventually also have other hardening tasks, including adding clamav, snort, and an ips, perhaps ossec, and will add cockpit

Requirements
------------

`ansible-galaxy install https://github.com/millerthegorilla/ansible-collection-hardening,os_immutable_fs`

Role Variables
--------------

The variables for devsec_hardening roles are not changeable.  They are included here for
completeness.

- os_hardening
```
  os_auditd_enabled: false
  os_immutable_fs: true
```
- ssh_hardening
```
  ssh_immutable_fs: true
  ssh_authorized_keys_file: '.ssh/authorized_keys.d/ignition'
```

Dependencies
------------

`https://github.com/millerthegorilla/ansible-collection-hardening,os_immutable_fs`

Example Playbook
----------------

```
- name: Devsec_os_hardening
  vars:
    os_auditd_enabled: false
    os_immutable_fs: true
  ansible.builtin.include_role:
    name: devsec.hardening.os_hardening
  tags: [devsec, devsec_os_hardening]

- name: Devsec_ssh_hardening
  vars:
    ssh_immutable_fs: true
    ssh_authorized_keys_file: '.ssh/authorized_keys.d/ignition'
  ansible.builtin.include_role:
    name: devsec.hardening.ssh_hardening
  tags: [devsec, devsec_ssh_hardening]
```

License
-------

Mit

Author Information
------------------

James Miller - https://github.com/millerthegorilla
