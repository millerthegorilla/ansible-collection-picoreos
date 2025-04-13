ssh_config
=========

A simple role to create .ssh/config file for all the hosts before connecting to remotes
The existing .ssh/config is moved to .ssh/config.old

Requirements
------------

A valid inventory file, optionally specifying ansible_user.

Role Variables
--------------
```
  ssh_config_out_path: "~/.ssh"
  ssh_config_accepted_key_type: "ssh-ed25519"
  ssh_config_id_file: "id_ed25519"
  ssh_config_coreos_ssh_path: "{{ ssh_config_out_path }}/{{ ssh_config_id_file }}"
```

Dependencies
------------

None

Example Playbook
----------------

```
- name: Create .ssh/config
  hosts: all:!localhosts
  connection: local
  roles:
    - role: ssh_config
      vars:
        ssh_config_out_path: "~/.ssh"
        ssh_config_accepted_key_type: "ssh-ed25519"
        ssh_config_id_file: "id_ed25519"
        ssh_config_coreos_ssh_path: "{{ ssh_config_out_path }}/{{ ssh_config_id_file }}"
  tags: "ssh_config"
```

License
-------

Mit

Author Information
------------------

James Miller - https://github.com/millerthegorilla
