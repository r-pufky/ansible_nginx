# [NGINX][h]
NGINX installation from public release tarball.

## [Requirements][i]
Requires [r_pufky.srv][g] galaxy-ng collection. See
[additional documentation][m] and [reference documentation][h] for
troubleshooting and config variables.

Install size: ~4MB

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
All [NGINX modules][o] are installed using the NGINX repository. CA
certificates are always installed.

### WARNING
> Debian service does NOT [automatically drop privileges][p] to managed user.
>
> Requires adding the following lines to nginx.conf:
>
>   ``` ini
>   user <user> [group]
>   ```

Path                     | Usage
-------------------------|-------
/etc/nginx/conf.d        | nginx_cfg_conf_d always deployed here.
/etc/nginx/secure.conf.d | nginx_cfg_secure_conf_d always deployed here.
/usr/share/nginx         | nginx_cfg_web_d always deployed here.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag              | Notes
 ------|-------------------|-------
  1    | nginx_flg_install | Install required packages, users, etc.
  2    | nginx_flg_config  | Install user-defined config.

### Example Playbooks

### New Deployment

``` yaml
- name: 'Default NGINX server with default example pages.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.nginx'
```

``` yaml
- name: 'Default NGINX server with additional modules installed.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.nginx'
  vars:
    nginx_srv_packages:
      - 'nginx-module-geoip'
      - 'nginx-module-xslt'
      - 'nginx-module-image-filter'
      - 'nginx-module-njs'
```

### Static Deployments
Most NGINX deployments will statically deploy configuration.

NGINX configuration is complex and nuanced. See [reference documentation][h]
keeping in mind where the [role places files](#usage).

``` yaml
- name: 'NGINX custom site with tightened service defaults.'
  ansible.builtin.inlcude_role:
    name: 'r_pufky.srv.nginx'
  vars:
    nginx_flg_config: true
    nginx_srv_packages:
      - 'nginx-module-geoip'
      - 'nginx-module-xslt'
      - 'nginx-module-image-filter'
      - 'nginx-module-njs'
    nginx_srv_restart: true
    nginx_srv_default: 'host_vars/nginx.example.com/nginx-secured'
    nginx_srv_default_debug: 'host_vars/nginx.example.com/nginx-debug-secured'
    nginx_cfg_conf_d: 'host_vars/nginx.example.com/conf.d'
    # Certificates, htpasswd should be placed here and referenced.
    nginx_cfg_secure_conf_d: 'host_vars/nginx.example.com/secure.conf.d'
    nginx_cfg_nginx_conf: 'host_vars/nginx.example.com/nginx.conf'
    nginx_cfg_web_d: 'host_vars/nginx.exmaple.com/webroot'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

  Release | Debian | Ansible | NGINX   | Notes
 ---------|--------|---------|---------|-------
  3.x.x   | 13     | 2.20    | v1.29.x | Ansible 2.20, feature flags, and semantic versioning.
  2.x.x   | 12     | 2.18    | v1.28.x | Migrate to Debian Trixie.
  1.x.x   | 12     | 2.12    | v1.26.x | Migrate from private repository

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_nginx/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_srv
[h]: https://nginx.org/en/docs/
[i]: https://github.com/r-pufky/ansible_nginx/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_nginx/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_nginx/blob/main/defaults/main/ports.yml
[m]: http://r-pufky.github.io/docs/service/nginx
[o]: https://nginx.org/en/linux_packages.html#Debian
[p]: https://nginx.org/en/docs/ngx_core_module.html#user