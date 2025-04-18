Role Name
=========

This role installs the basic requirements for ansible to function as well as some client libraries that are necessary for the later roles to use, such as those for selinux and firewalld.

You can specify the variable `req_install_other_packages` either in your playbook or as extra_vars on the command line.  This will add packages to the install.  This can help if you
want to speed up the other roles, as whenever they install a package the machine has to reboot.
Adding the packages here will mean only one reboot, and since a reboot takes about 4 minutes, the
roles then run more quickly.  To do this for this collection you can try:
`ansible-playbook millerthegorilla.picoreos.picoreos_pb -i /home/user/inventory --ask-vault-pass --extra-vars "req_install_other_packages='fail2ban openvpn openssh'"`

## firewall

After installing python etc, and when the machine has rebooted, the role adds a rich rule to the firewall configuration to limit access to ssh by only the local network.
You can skip this with the tag `firewall_ssh`.
The rich rule is as follows:
`'rule family="ipv4" source address="192.168.1.0/24" port protocol="tcp" port="22" accept'`
After adding this rich rule, another firewall-cmd sees ssh is dropped.

## tags

`firewall` and `req_install_firewall` tags are available on all firewall tasks.
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
If you are running this role before a series of other roles, and those roles install packages, you can
install them here in advance to minimise the amount of reboots required across the run of the playbook.
```
 req_install_required_packages: "fail2ban openvpn openssh"
```
### caveat
Currently, due to rpm-ostree systems using nss-altfiles, if you add the openvpn package, then you will need to add it to /etc/group manually or later package layering will fail.  (see https://github.com/coreos/rpm-ostree/issues/49#issuecomment-478091562).  So if you add openvpn you will need to add the
following task after the req_install role...
```
- name: Openvpn requires manual addition to /etc/group or later package layering fails
  hosts: picoreos_hosts
  gather_facts: false
  become: true
  tasks:
    - name: Add openvpn group to /etc/group
      ansible.builtin.command:
        cmd: bash -c "grep -E '^openvpn:' /usr/lib/group >> /etc/group"
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
