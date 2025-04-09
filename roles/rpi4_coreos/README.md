Role Name
=========

This role installs Fedora Coreos for raspberry pi 4b using the coreos-installer script and then installs the latest EDK firmware.  You will need a microsd card.

## caveat!
<b>MAKE CERTAIN</b> that the disk name is correct in the [variables](#role-variables) for the rpi4_coreos role.

The rpi4_coreos role is a <b>destructive operation</b> and will overwrite anything that is on the target disk.

You will need to put the microssd card into a slot on your machine, and then define which disk name it is in the playbook as the variable to the rpi4_coreos role.  The rpi4_coreos role will run to install the latest coreos and firmware and will then prompt you to remove the microsd card and place it in your raspberry pi, and boot up.

As soon as the pi has finished booting, which you can confirm with ```ssh core@picoreos.lan```, you can press enter to continue.  All roles/tasks from then on will run automatically, unless the script detects that you need to adjust sudo settings on the remote.

There are a few reboots during the process of installation and these take some time, as coreos on the pi4b takes about 4 minutes to reboot.

Requirements
------------

Create a new python virtual environment and install ansible and rpm in the prefix.  Then layer or install python3-rpm onto the host.

Tested using Fedora Silverblue as a control node.

It is necessary to create an ssh key file using ssh-keygen.  The following command will work:
```
ssh-keygen -t ed25519 -C your_comment_here
```
Make sure to use a secure password.  The files id_ed25519 and id_ed25519.pub will be created unless you
specify different filenames.  You will need to tell the rpi4_coreos role where to find the public keyfile.
See the rpi4_coreos section of [variables](#variables).  This is used in the butane file so that the
ssh key is provisioned into the microssd image.



## optional
You can layer/install coreos-installer and ansible will use it.  If it doesn't find it, it uses a version inside a container instead.

### butane
You can find the butane template in roles/rpi4_coreos/templates/.  Coreos has an immutable file system and so currently /var is mounted within a luks encrypted partition.  If you want to unlock this partition on another machine you will need to define an encryption key in the butane configuration.

Role Variables
--------------

```
  rpi4_coreos_provision_disk: "sdb"
  rpi4_coreos_ssh_path: '~/.ssh/id_ed25519.pub'
```

Dependencies
------------

You will need to install python3-rpm on the control node, as well as the ansible and rpm libraries in a virtual environment prefix.

Example Playbook
----------------
```
- name: Provision CoreOS on Raspberry Pi 4
  hosts: localhost
  gather_facts: true
  pre_tasks:
    - name: Include vars
      ansible.builtin.include_vars:
        file: ./roles/rpi4_coreos/vars/main.yaml
  roles:
    - role: rpi4_coreos
      rpi4_coreos_provision_disk: "sdb"
      rpi4_coreos_ssh_path: '~/.ssh/id_ed25519.pub'
      when: "(rpi4_coreos_provision_disk in ansible_devices) | bool"
      tags: rpi4_coreos
```
License
-------

MIT

Author Information
------------------

James Miller.  https://github.com/millerthegorilla/picoreos
