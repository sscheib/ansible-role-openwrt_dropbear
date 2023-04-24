openwrt_dropbear
=========

Configure dropbear on OpenWrt devices using UCI and deploy SSH keys for passwordless authentication with dropbear.

Requirements
------------

None

Role Variables
--------------
```
- dropbear_options: UCI options to apply to dropbear
  Default value:
  dropbear_options:
    enable: true
    verbose: true
    BannerFile: '/etc/issue.net'
    PasswordAuth: false
    Port: 605
    RootPasswordAuth: false
    RootLogin: true
    GatewayPorts: false
    keyfile: '/etc/dropbear/dropbear_ed25519_host_key'
    SSHKeepAlive: 300
    IdleTimeout: 0
    mdns: false
    MaxAuthTries: 3
    Interface:

- dropbear_authorized_keys: authorized keys to deploy for accessing the device using dropbear

- dropbear_service_name: dropbear service name
  Default value:
  _def_dropbear_service_name: 'dropbear'
  
- dropbear_authorized_keys_path: path to dropbear's authorized_keys file
  Default value:
  _def_dropbear_authorized_keys_path: '/etc/dropbear/authorized_keys'

- dropbear_delete_unspecified_ssh_keys: whether to delete unspecified SSH keys from the authorized_keys file
  Default value: false

- dropbear_deploy_hotplug_script: whether to deploy the hotplug script which will take care to reload dropbear if an interface changes its status (to prevent crashing)
  Default value: true

- dropbear_hotplug_directory: path to the hotplug directory of OpenWrt
  Default value: '/etc/hotplug.d/iface'

- dropbear_hotplug_directory_owner: owner of the hotplug directory
  Default value: 'root'

- dropbear_hotplug_directory_group: group of the hotplug directory
  Default value: 'root'

- dropbear_hotplug_directory_mode: chmod f the hotplug directory
  Default value: '0755'

- dropbear_hotplug_script_src: source Jinja2 template for the hotplug script
  Default value: 'etc_hotplug.d_dropbear.j2'

- dropbear_hotplug_script_dest: destination path of the hotplug script
  Default value: '/etc/hotplug.d/iface/99-dropbear'

- dropbear_hotplug_script_owner: owner of the hotplug script
  Default value: 'root'

- dropbear_hotplug_script_group: group of the hotplug script
  Default value: 'root'

- dropbear_hotplug_script_mode: chmod of the hotplug script
  Default value: '0600'
```

Dependencies
------------

None

Example Playbook
----------------

```
- name: 'Configure dropbear on OpenWrt'
  hosts: 'all'
  gather_facts: false
  vars:
    dropbear_options:
      enable: true
      verbose: false
      BannerFile:
      PasswordAuth: false
      Port: 2222
      RootPasswordAuth: false
      RootLogin: true
      GatewayPorts: false
      keyfile: '/etc/dropbear/dropbear_ed25519_host_key'
      SSHKeepAlive: 300
      IdleTimeout: 0
      mdns: false
      MaxAuthTries: 3
      Interface:
    dropbear_authorized_keys:
      - 'my_key.pub'
      - 'another_key.pub'
    dropbear_delete_unspecified_ssh_keys: true
  roles:
    - name: 'openwrt_dropbear'
```

Please note: To unset dropbear options the respective key should be defined without any value (as shown in the above example for they key `BannerFile`).

License
-------

GPLv2 or later
