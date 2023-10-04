openwrt_dropbear
=========

Configure dropbear on OpenWrt devices using UCI and deploy SSH keys for passwordless authentication with dropbear.

Requirements
------------

None

Role Variables
--------------
| variable                                 | default                            | required | description                                                                                                                           |
| :--------------------------------------- | :--------------------------------- | :------- | :------------------------------------------------------------------------------------------------------------------------------------ |
| `dropbear_options`                       | See `defaults/main.yml`            | false    | UCI options to apply to dropbear                                                                                                      |
| `dropbear_authorized_keys`               | Unset                              | false    | authorized keys to deploy for accessing the device using dropbear                                                                     |
| `dropbear_service_name`                  | `dropbear`                         | false    | dropbear service name                                                                                                                 |
| `dropbear_authorized_keys_path`          | `/etc/dropbear/authorized_keys`    | false    | path to dropbear's `authorized_keys` file                                                                                             |
| `dropbear_delete_unspecified_ssh_keys`   | false                              | false    | whether to delete unspecified SSH keys from the `authorized_keys` file                                                                |
| `dropbear_deploy_hotplug_script`         | true                               | false    | whether to deploy the hotplug script which will take care to reload dropbear if an interface changes its status (to prevent crashing) |
| `dropbear_hotplug_directory`             | `/etc/hotplug.d/iface`             | false    | path to the hotplug directory of OpenWrt                                                                                              |
| `dropbear_hotplug_directory_owner`       | `root`                             | false    | owner of the hotplug directory                                                                                                        |
| `dropbear_hotplug_directory_group`       | `root`                             | false    | group of the hotplug directory                                                                                                        |
| `dropbear_hotplug_directory_mode`        | `0755`                             | false    | mode of the hotplug directory                                                                                                         |
| `dropbear_hotplug_script_src`            | `etc_hotplug.d_dropbear.j2`        | false    | source Jinja2 template for the hotplug script                                                                                         |
| `dropbear_hotplug_script_dest`           | `/etc/hotplug.d/iface/99-dropbear` | false    | destination path of the hotplug script                                                                                                |
| `dropbear_hotplug_script_owner`          | `root`                             | false    | owner of the hotplug script                                                                                                           |
| `dropbear_hotplug_script_group`          | `root`                             | false    | group of the hotplug script                                                                                                           |
| `dropbear_hotplug_script_mode`           | `0600`                             | false    | mode of the hotplug script                                                                                                            |

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
