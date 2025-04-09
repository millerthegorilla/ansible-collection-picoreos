NordVpn
=========

This role installs openvpn, and configures it by adding `update-resolv-conf` and `update-systemd-resolved`
plus a systemd boot service file that randomly selects a new vpn definition file every boot.
In order to do this, a number of client files are copied over to the remote machine, to be placed in `/etc/openvpn/client/`.  Currently, only UK server client definition files are used.  If you want to use a different location, you will need to copy the client files for your location and update them to work with this setup by adding

```
up /etc/openvpn/update-systemd-resolved
down /etc/openvpn/update-systemd-resolved
```
 and
```
auth-user-pass /etc/openvpn/auth.txt
auth-nocache
```
to each conf file in the client directory, perhaps with a bit of bash magic...

Requirements
------------

The openvpn configuration by Nordvpn requires you to leave an access token in a file called auth.txt
which is located at /etc/openvpn.  This file contains two lines, which are two halves of the token.
You can generate the token at nordvpn.com by following the instructions here:
https://support.nordvpn.com/hc/en-us/articles/20285620014353-Linux-start-on-boot-manual-connection

Role Variables
--------------
The access token keys are taken from the following variables, which you can define per host or in a playbook.

```
  nordvpn_openvpn_auth_txt_upper: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    30356239336663323737323661613237336131643664306231316263323439396330393861386366
    6166333065336561353035393633626538666630616263350a303066313866333534343863636133
    37613130353761383062363264376535386337663666326563633263336134393530396463616462
    3730646130656437370a653434323037656365633564383232353837313265313039653730346465
    39353338366334336639306438663164
  nordvpn_openvpn_auth_txt_lower: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    37343364336161333235343133396130306663363363613938613732376139373165393539323535
    3039646539303162316663643338353764356530656161360a656136383038303566346430616331
    36656432646638313565333132633239353130653838306165356237623666373838386564636532
    3264383162393332310a343731393334306630353932643330363261316465356636613263616437
    63333964333832356364663165663163
  ```

Dependencies
------------

The role installs the openvpn package onto the machine, and reboots.

Example Playbook
----------------

- name: Add VPN
  hosts: picoreos_hosts
  gather_facts: true
  become: true
  roles:
    - role: nordvpn
      tags: nordvpn
      vars:
          nordvpn_openvpn_auth_txt_upper: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            30356239336663323737323661613237336131643664306231316263323439396330393861386366
            6166333065336561353035393633626538666630616263350a303066313866333534343863636133
            37613130353761383062363264376535386337663666326563633263336134393530396463616462
            3730646130656437370a653434323037656365633564383232353837313265313039653730346465
            39353338366334336639306438663164
          nordvpn_openvpn_auth_txt_lower: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            37343364336161333235343133396130306663363363613938613732376139373165393539323535
            3039646539303162316663643338353764356530656161360a656136383038303566346430616331
            36656432646638313565333132633239353130653838306165356237623666373838386564636532
            3264383162393332310a343731393334306630353932643330363261316465356636613263616437
            63333964333832356364663165663163
License
-------

MIT

Author Information
------------------

James Miller.  https://github.com/millerthegorilla/picoreos
