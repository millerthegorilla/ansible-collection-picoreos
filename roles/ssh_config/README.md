ssh_config
=========

A simple role to create .ssh/config file for all the hosts before connecting to remotes
The existing .ssh/config is moved to .ssh/config.old
The following fragment is created *per host*:
```
Host picoreos
  Hostname picoreos
  User core
  PubKeyAuthentication yes
  PubkeyAcceptedKeyTypes=ssh-ed25519
  PreferredAuthentications publickey
  IdentityFile /home/your_username_home_directory_here/.ssh/id_ed25519
  IdentitiesOnly yes
```
The fragments are then assembled and copied to become .ssh/config

Requirements
------------

A valid inventory file, optionally specifying ansible_user.
```
localhosts:
  hosts:
    localhost:
      ansible_connection: local
      ansible_python_interpreter: '{{ ansible_playbook_python }}'
      ansible_become_method: sudo
      ansible_become_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          38616366623036323833373530373966306165616464343463393636313032366662373638613330
          6462613733333264646337646338663462313437373462650a636166653161656266363736656334
          33666437303731353566333033623732633335646633663930313866313031643665373462366162
          3065613132643437630a303161353232393265663361366237633633356138356365636333373661
          3764393831316234666316164663537393335616533346664383366
picoreos_hosts:
  hosts:
    picoreos1:
      ansible_user: core
      ansible_become_user: root
      ansible_become_method: sudo
      ansible_python_interpreter: /usr/bin/python
      ansible_become_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          33653166616232343661613064353065313564613465333262373866643666613065326264306662
          3663613134613038356532333861353032633238653430650a393164383264333962393266376235
          39653661333566323862663238373738663234383162613937393432613561383231363430313930
          6465646531383561640a64666162663434303438313039616434373336613331323265
          6437
```

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
