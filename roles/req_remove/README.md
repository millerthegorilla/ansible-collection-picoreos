Remove ansible requirements
===========================

This role removes the packages added by the req_install role, that are necessary for ansible to function.  Coreos doesn't include these files as it is primarily designed to serve up containers
using podman.

Note that ansible will cease to function after this role has finished (it ends with a reboot), so this role should probably be your last role defined in your playbook.

Requirements
------------

None

Role Variables
--------------

The packages defined by req_install are imported from the defaults/main.yml of the req_install
role.  You can override them here.  If you override the `req_install_ansible_required_packages`
in the req_install role, then you should probably override it in your playbook that uses this role.

```
req_install_ansible_required_packages: python python3-libselinux python3-rpm python3-firewall
                                       policycoreutils-python-utils firewalld
```

Dependencies
------------

None.

Example Playbook
----------------

```
- name: Remove required packages
  hosts: picoreos_hosts
  gather_facts: true
  become: true
  roles:
    - role: req_remove
    - tags: req_remove
```

License
-------

Mit

Author Information
------------------

James Miller - https://github.com/millerthegorilla
