Role Name
=========

This role installs the basic requirements for ansible to function as well as some client libraries that are necessary for the later roles to use, such as those for selinux and firewalld.  You might argue that those packages necessary for those roles should be installed by those roles, and I agree, with the exception that rpm-ostree as used by coreos requires a reboot and takes ages, so I have included them in the same place.
Moving them is on my list of things todo.

Requirements
------------

The role uses the ansible.builtin.raw module for most of the work.  It is idempotent though, so you can run it repeatedly without issue.

`gather_facts = false` is a requirement of the role, as until it has finished, there is no python installed on the machine.

Role Variables
--------------

It is necessary to specify the Fedora version that is being used.
```
 fedora_version: 41
```

Dependencies
------------

This role requires a remote machine running Fedora Coreos.  Tested with Fedora 41 on a raspberry pi 4b 8gb.

Example Playbook
----------------
```
- name: Install required packages
  hosts: picoreos_hosts
  gather_facts: false
  remote_user: core
  pre_tasks:
    - name: Include vars
      ansible.builtin.include_vars:
        file: ./roles/req_install/vars/main.yaml
  roles:
    - role: req_install
      vars:
        fedora_version: 41
      tags: req_install
```
License
-------

MIT

Author Information
------------------

James Miller.  https://github.com/millerthegorilla/picoreos
