<!-- markdownlint-disable MD013 MD041 -->
[![ansible-lint](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/ansible-lint.yml) [![Publish to Ansible Galaxy](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/release.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/release.yml) [![markdown link check](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/markdown-link-check.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/markdown-link-check.yml) [![markdownlint](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/markdownlint.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/markdownlint.yml) [![pyspelling](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/pyspelling.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/pyspelling.yml) [![commitlint](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/commitlint.yml/badge.svg)](https://github.com/sscheib/ansible-role-openwrt_dropbear/actions/workflows/commitlint.yml)

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit) [![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-%23FE5196?logo=conventionalcommits&logoColor=white)](https://conventionalcommits.org) [![License: GPL v2](https://img.shields.io/badge/License-GPL_v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
<!-- markdownlint-disable MD013 MD041 -->

## openwrt_dropbear

Configure `dropbear` on `OpenWrt` devices using `UCI` and deploy SSH keys for passwordless authentication with `dropbear`

## Requirements

This role uses the `authorized_key` from the collection [`ansible.posix`](https://github.com/ansible-collections/ansible.posix). The collection is specified via
`collections/requirements.yml`.

## Role Variables

| variable                                 | default                            | required | description                                                                                                                           |
| :--------------------------------------- | :--------------------------------- | :------- | :------------------------------------------------------------------------------------ |
| `dropbear_options`                       | See `defaults/main.yml`            | false    | `UCI` options to apply to `dropbear`                                                  |
| `dropbear_authorized_keys`               | unset                              | false    | Authorized SSH keys to deploy for accessing the device using `dropbear`               |
| `dropbear_service_name`                  | `dropbear`                         | false    | `dropbear` service name                                                               |
| `dropbear_authorized_keys_path`          | `/etc/dropbear/authorized_keys`    | false    | Path to the `authorized_keys` file of `dropbear`                                      |
| `dropbear_delete_unspecified_ssh_keys`   | false                              | false    | Whether to delete unspecified SSH keys from the `authorized_keys` file                |
| `dropbear_deploy_hotplug_script`         | true                               | false    | Whether to deploy the `hotplug` script which will reload `dropbear` [^interface]      |
| `dropbear_hotplug_directory`             | `/etc/hotplug.d/iface`             | false    | Path to the `hotplug` directory of `OpenWrt`                                          |
| `dropbear_hotplug_directory_owner`       | `root`                             | false    | Owner of the `hotplug` directory                                                      |
| `dropbear_hotplug_directory_group`       | `root`                             | false    | Group of the `hotplug` directory                                                      |
| `dropbear_hotplug_directory_mode`        | `0755`                             | false    | Mode of the `hotplug` directory                                                       |
| `dropbear_hotplug_script_src`            | `etc_hotplug.d_dropbear.j2`        | false    | Source `Jinja2` template for the `hotplug` script                                     |
| `dropbear_hotplug_script_dest`           | `/etc/hotplug.d/iface/99-dropbear` | false    | Destination path of the `hotplug` script                                              |
| `dropbear_hotplug_script_owner`          | `root`                             | false    | Owner of the `hotplug` script                                                         |
| `dropbear_hotplug_script_group`          | `root`                             | false    | Group of the `hotplug` script                                                         |
| `dropbear_hotplug_script_mode`           | `0600`                             | false    | Mode of the `hotplug` script                                                          |

## Dependencies

None

## Example Playbook

```yaml
---
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
...
```

**Please note**:

- To unset `dropbear` options the respective key should be defined without any value (as shown in the above example for they key `BannerFile`).

## Contributing

First off, thanks for taking the time to contribute! ❤️

All types of contributions are encouraged and valued.
Please See the [`CONTRIBUTING.md`](CONTRIBUTING.md) for different ways to help and details about how this project handles them.

## License

[`GPL-2.0-or-later`](LICENSE)

[^interface]: The `hotplug` script reloads `dropbear` when an interface change is observed (interfaces goes up/down). This is done to prevent a crash in `dropbear`
              as well as to only listen on actually existing interfaces.
