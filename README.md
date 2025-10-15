# NGINX
NGINX installation from public release tarball.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_nginx/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_nginx/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_nginx/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## Example Playbook
Read defaults documentation.

Install NGINX using custom root certificates, configuration directory, and file
from the ansible controller.

``` yaml
- name: 'nginx server'
  hosts: 'nginx.example.com'
  become: true
  roles:
     - 'r_pufky.srv.nginx'
  vars:
    nginx_cfg_ca_dir: 'host_vars/nginx.example.com/data/ca'
    nginx_cfg_confd_dir: 'host_vars/nginx.example.com/data/conf.d'
    nginx_cfg_nginx_conf_file: 'host_vars/nginx.example.com/data/nginx.conf'
```

Install NGINX with auto-managed basic authentication.

``` yaml
- name: 'nginx server'
  hosts: 'nginx.example.com'
  become: true
  roles:
     - 'r_pufky.srv.nginx'
  vars:
    nginx_cfg_basic_auth_enable: true
    nginx_cfg_basic_auth_users:
      - user: 'test'
        pass: 'test'
      - user: 'test2'
        state: 'absent'
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_docs/blob/main/ansible/environment.md)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12.0.0-2.0.3-1.0.0`

* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_nginx)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_nginx/tree/12.x)**: 12 Bookworm.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_nginx/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
